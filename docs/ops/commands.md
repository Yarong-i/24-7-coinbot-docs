# Ops Commands — 운영 점검/튜닝 치트시트


> 실운영에서 쓰던 점검 루틴을 한 파일에 모았습니다.


## 0) 기준시각(T0) 세팅
```bash
# 없으면 최근 7시간 전으로
[[ -z "$T0" ]] && export T0=$(date -u -d '7 hours ago' +%FT%TZ)
echo "T0=$T0"

1) 실체결 성과 (fills / cycles)

for S in BTC SOL; do
echo "=== $S since $T0 ==="
sudo -u coinbot /usr/local/bin/bot-pnl --since "$T0" --symbol $S
sudo -u coinbot /usr/local/bin/bot-pnl --since "$T0" --symbol $S --cycles
done

2) 빈도·효율 요약 (cycles/h, PF)

HRS=$(awk -v s="$(date -u -d "$T0" +%s)" -v n="$(date -u +%s)" 'BEGIN{printf "%.2f",(n-s)/3600}')
for S in BTC SOL; do
CYC=$(sudo -u coinbot /usr/local/bin/bot-pnl --since "$T0" --symbol $S --cycles \
| awk -F'[, ]+' '/cycles/{print $4; exit}')
PF=$(sudo -u coinbot /usr/local/bin/bot-pnl --since "$T0" --symbol $S --cycles \
| awk '{for(i=1;i<=NF;i++) if($i=="PF"){print $(i+1); exit}}' | sed 's/[^0-9.\-]//g')
CYH=$(awk -v c="$CYC" -v h="$HRS" 'BEGIN{if(h>0) printf "%.2f", c/h; else print "0.00"}')
echo "$S | cycles=$CYC | cycles/h=$CYH | PF=$PF"
done

3) 알림/에러 로그 스캔 (T0 이후)
sudo journalctl -u coinbot-position-notifier.service \
-S "@$(date -u -d "$T0" +%s)" --no-pager \
| egrep -i 'New trade detected|진입|청산|flip|error' || true

4) 프로세스에 파라미터 실제 반영 확인
for s in btc sol; do
pid=$(systemctl show -p MainPID coinbot@$s.service | cut -d= -f2)
echo "[$s] pid=$pid"
sudo cat /proc/$pid/environ | tr '\0' '\n' \
| egrep '^(TIMEFRAME|LEVERAGE|ATR_GATE_MULT|ENTRY_RETEST_TOL_PCT|ENTRY_RETEST_WINDOW_S|ENTRY_COOLDOWN_S)='
done

5) 설정 변경 & 재시작 (예시)
# BTC: ATR 게이트 조정
sudo sed -i -E 's/^(ATR_GATE_MULT)=.*/\1=1.20/' /etc/default/coinbot-btc
sudo systemctl restart coinbot@btc.service


# SOL: 변경 전 백업
sudo bash -lc 'f=/etc/default/coinbot-sol; cp -a "$f"{,.bak.$(date +%s)}'
# SOL: 리테스트/쿨다운/타임프레임/ATR
sudo sed -i -E 's/^(ENTRY_RETEST_WINDOW_S)=.*/\1=150/' /etc/default/coinbot-sol
sudo sed -i -E 's/^(ENTRY_RETEST_TOL_PCT)=.*/\1=0.08/' /etc/default/coinbot-sol
sudo sed -i -E 's/^(ENTRY_COOLDOWN_S)=.*/\1=420/' /etc/default/coinbot-sol
sudo sed -i -E 's/^(ATR_GATE_MULT)=.*/\1=1.12/' /etc/default/coinbot-sol
sudo sed -i -E 's/^(TIMEFRAME)=.*/\1=3m/' /etc/default/coinbot-sol
sudo systemctl restart coinbot@sol.service


재시작 묶음 스크립트
scripts/restart-services.sh

---


## 📄 docs/infra/notifier.md


```md
# Position Notifier — 아키텍처 & 배포


봇이 로컬 UDS(Unix Domain Socket)에 **체결/상태** 이벤트를 쓰면, 브리지가 **필터링/보강 후 Discord**로 전송합니다.


## 아키텍처
[coinbot@{btc,sol}] --(JSON over UDS)--> [/run/coinbot/notify.sock] | | | Notifier | (필터/보강/재시도) -------------------------------------> Discord Webhook
### 주요 기능
- **화이트리스트**: 상태/시작/이어받음/경고/청산은 항상 통과
- **ENTRY_NOTIFY_MODE=all**: 진입 문구 누락 방지(과거 `filled` 드롭 문제 개선)
- **STATUS_ENRICH**: ccxt 프로브로 포지션/최근 체결/열린 주문/판단을 상태 핑에 주입
- **CONFIRM_EXCHANGE**: 엔트리/엑싯은 거래소 실체결 확인 시만 전송
- **Retry**: 네트워크 오류시 지수백오프 재시도


## 메시지 포맷(예시)
```json
{
"ts": "2025-09-06T09:15:30Z",
"type": "entry", // entry|exit|status|warn|liquidation|flip
"symbol": "SOLUSDT",
"side": "LONG",
"price": 123.45,
"qty": 0.80,
"orderId": "1234567890",
"extra": {"timeframe": "3m", "leverage": 3}
}

배포 파일

systemd/coinbot-position-notifier.service — 서비스 유닛

configs/coinbot-notifier.env.example — 환경변수 예시

scripts/send_test_notify.py — 로컬 소켓으로 테스트 전송

notifier/notifier.py — 최소 동작 참조 구현(포트폴리오용)

설치/실행
# 1) 바이너리/의존성 (requests, ccxt 선택)
sudo pip3 install requests ccxt || true


# 2) 환경파일 배치
sudo install -d -m 0755 /etc/default
sudo install -m 0644 configs/coinbot-notifier.env.example /etc/default/coinbot-notifier
# → DISCORD_WEBHOOK_URL 등 채워넣기


# 3) 소켓 디렉토리
sudo install -d -o coinbot -g coinbot -m 0755 /run/coinbot || true


# 4) systemd 등록
sudo install -d /usr/local/share/coinbot/notifier
sudo install -m 0755 notifier/notifier.py /usr/local/share/coinbot/notifier/notifier.py
sudo install -m 0644 systemd/coinbot-position-notifier.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable --now coinbot-position-notifier.service


# 5) 동작 확인
journalctl -u coinbot-position-notifier.service -f
python3 scripts/send_test_notify.py # 테스트 페이로드 전송

🧰 scripts/restart-services.sh
#!/usr/bin/env bash
set -Eeuo pipefail
sudo systemctl daemon-reload || true
for s in btc sol; do sudo systemctl restart coinbot@${s}.service || true; done
sudo systemctl restart coinbot-position-notifier.service || true
echo "restarted: coinbot@btc, coinbot@sol, coinbot-position-notifier"

🧪 scripts/send_test_notify.py
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import os, socket, json, time
SOCK = os.environ.get("NOTIFY_SOCKET", "/run/coinbot/notify.sock")
msg = {
"ts": time.strftime("%Y-%m-%dT%H:%M:%SZ", time.gmtime()),
"type": "entry",
"symbol": "BTCUSDT",
"side": "LONG",
"price": 60000.0,
"qty": 0.001,
"orderId": "TEST-ORDER-1",
}
with socket.socket(socket.AF_UNIX, socket.SOCK_STREAM) as s:
s.connect(SOCK)
s.sendall((json.dumps(msg) + "\n").encode())
print("sent test message to", SOCK)

🧩 systemd/coinbot-position-notifier.service
[Unit]
Description=Coinbot position notifier bridge
After=network-online.target
Wants=network-online.target


[Service]
Type=simple
User=coinbot
Group=coinbot
EnvironmentFile=/etc/default/coinbot-notifier
ExecStart=/usr/bin/python3 /usr/local/share/coinbot/notifier/notifier.py
Restart=always
RestartSec=5s
SyslogIdentifier=coinbot-position-notifier


[Install]
WantedBy=multi-user.target

🔑 configs/coinbot-notifier.env.example
# Notifier 환경 예시(실 값은 서버에서만 설정하고 깃에는 올리지 마세요)
DISCORD_WEBHOOK_URL=https://discord.com/api/webhooks/xxxx/xxxx
NOTIFY_SOCKET=/run/coinbot/notify.sock
ENTRY_NOTIFY_MODE=all # all|filled
STATUS_ENRICH=1 # 1: ccxt로 상태 주입
CONFIRM_EXCHANGE=1 # 1: 실체결 확인 후 전송
EXCHANGE=BINANCE
API_KEY=__redacted__
API_SECRET=__redacted__
TIMEOUT_S=10
MAX_RETRY=3

🐍 notifier/notifier.py
#!/usr/bin/env python3
"fields": fields
}]}


async def handle(reader: asyncio.StreamReader, writer: asyncio.StreamWriter):
peer = writer.get_extra_info('peername')
try:
data = await reader.readline()
if not data:
return
msg = json.loads(data.decode().strip())
t = str(msg.get("type", "")).lower()
# filtering
if t not in ALWAYS_PASS:
if t == "entry" and ENTRY_MODE != "all":
return
if not WEBHOOK:
print("WEBHOOK not set; drop", file=sys.stderr)
return
if not await confirm_order(msg):
print("exchange confirm failed; drop", file=sys.stderr)
return
# enrich (optional)
if STATUS_ENRICH and ex and t == "status" and msg.get("symbol"):
try:
pos = ex.fetch_positions_risk([msg["symbol"]])
msg.setdefault("extra", {})["position_size"] = pos[0].get("contracts") if pos else None
except Exception as e:
print(f"enrich failed: {e}", file=sys.stderr)
await post_discord(to_embed(msg))
except Exception as e:
print(f"handle error: {e}", file=sys.stderr)
finally:
try:
writer.close(); await writer.wait_closed()
except Exception:
pass


async def main():
try:
os.unlink(SOCK)
except FileNotFoundError:
pass
os.makedirs(os.path.dirname(SOCK), exist_ok=True)
server = await asyncio.start_unix_server(handle, path=SOCK)
os.chmod(SOCK, 0o666) # 편의상; 운영에선 소유권/umask로 제한 권장
print(f"notifier listening on {SOCK}")
async with server:
await server.serve_forever()


if __name__ == "__main__":
asyncio.run(main())
