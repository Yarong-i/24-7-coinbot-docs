# Parameter Change Log

> 개인 연구용 자동매매(알고) 봇의 **파라미터 조정 이력**입니다.  
> 포트폴리오 목적의 기록이며, 투자 조언이 아닙니다.

| When (UTC)        | Symbol | Param                | Old  | New  | Reason / Evidence                                      | Link |
|-------------------|--------|----------------------|------|------|---------------------------------------------------------|------|
| 2025-09-06 09:20Z | BTC    | `ATR_GATE_MULT`      | 1.25 | 1.20 | cycles/h=0.00(목표≥0.20), 신호 과소. ATR% p80≈0.161     | —    |
| 2025-09-06 09:20Z | SOL    | `ENTRY_RETEST_WINDOW_S` | 150  | 120  | PF=0.00, 최근 1사이클 손실 -0.32 USDT → 오탐 억제 강화 | —    |
| 2025-09-06 09:20Z | SOL    | `ENTRY_RETEST_TOL_PCT`  | 0.08 | 0.07 | 리테스트 정밀도 상향                                     | —    |
| 2025-09-06 09:20Z | SOL    | `ATR_GATE_MULT`      | 1.12 | 1.10 | cycles/h=0.06<목표(≥0.30) → 약간 더 개방                 | —    |
| 2025-09-06 11:20Z | BTC    | `ATR_GATE_MULT`  | 1.25 | 1.20 | 11h 동안 cycles=0 → 신호 과소 추정(완화)     | —    |
| 2025-09-06 11:20Z | SOL    | `ATR_GATE_MULT`  | 1.12 | 1.10 | 11h 동안 cycles=0 → 빈도 소폭 상향            | —    |
| 2025-09-07 HH:MMZ | BTC | `ATR_GATE_MULT` | 1.20 | 1.10 | 24h cycles/h=0.00<0.20; ATR% p80≈0.129 → 신호 과소로 완화 | — |
| 2025-09-07 HH:MMZ | SOL | `ATR_GATE_MULT` | 1.10 | 1.06 | 24h cycles/h=0.09<0.30, PF↑; ATR% p80≈0.125 → 빈도 소폭↑ | — |
| 2025-09-08 HH:MMZ | BTC | `ATR_GATE_MULT` | 1.10 | 1.14 | 24h cycles/h≈0.17, PF=0.00 → 노이즈 컷 위해 소폭 강화(+3.6%, p80=0.091) | — |
| 2025-09-08 HH:MMZ | SOL | `ATR_GATE_MULT` | 1.06 | 1.02 | 24h cycles/h≈0.08≪목표, PF↑ → 빈도 보강 소폭 완화(−3.8%, p80=0.189)     | — |
| 2025-09-09 HH:MMZ | BTC | `ATR_GATE_MULT`   | 1.14 | 1.11 | 25h cycles/h=0.04<0.20, p80=0.117 → 기회 확대 위해 소폭 완화(−2.6%) | — |
| 2025-09-09 HH:MMZ | SOL | `ENTRY_COOLDOWN_S`| 420  | 360  | ATR=1.02(하한), 25h cycles/h=0.04≪목표 → 빈도 보강 위해 쿨다운 완화 | — |
| 2025-09-10 HH:MMZ | BTC | `ATR_GATE_MULT`       | 1.11 | 1.08 | 24h cycles/h=0.04<0.20, p80=0.131 → 기회 확대 위해 소폭 완화(−2.7%) | — |
| 2025-09-10 HH:MMZ | SOL | `ENTRY_RETEST_TOL_PCT`| 0.07 | 0.08 | ATR=1.02(하한), cycles/h=0.00 → 리테스트 허용폭 상향으로 후보 신호 확대 | — |
| 2025-09-11 HH:MMZ | BTC | `ATR_GATE_MULT`   | 1.08 | 1.05 | 22h cycles/h=0.00<0.20, p80=0.128 → 진입 억제 완화를 위해 추가 완화(−2.9%) | — |
| 2025-09-11 HH:MMZ | SOL | `ENTRY_COOLDOWN_S`| 360  | 300  | ATR=1.02(하한), cycles/h=0.00 → 빈도 보강 위해 쿨다운 완화 | — |
| 2025-09-12 HH:MMZ | ETH | `INIT_PARAMS`     | — | TIMEFRAME=3m,LEV=3,TOL=0.08,WIN=120,COOL=300,ATR=1.05 | 신규 편성(솔 슬롯 대체), 48h 관찰 시작 | — |
| 2025-09-12 HH:MMZ | SOL | `SERVICE_STATE`   | active | stopped | ETH 실험 위해 임시 중지(설정 보존) | — |
| 2025-09-13 01:52Z    | BTC    | `ATR_GATE_MULT`     | 1.05   | 1.25  | 31h cycles PF=0.60·PnL−0.16 → 노이즈 컷 위해 트리거 상향 **(+19%, p80=0.127)**               | —     |
| 2025-09-13 01:52Z    | BTC    | `ENTRY_COOLDOWN_S`  | 360    | 600   | 재진입 군집 억제·사이클 품질 개선 목적 **(+66%)**                                            | —     |
| 2025-09-13 01:45Z    | ETH    | `ATR_GATE_MULT`     | 1.05   | 1.10  | 초기 과진입 방지, 품질 우선 소폭 상향 **(+4.8%, p80=0.188)**                                 | —     |
| 2025-09-14 HH:MMZ | BTC | `ATR_GATE_MULT`           | 1.25 | 1.15 | 24h cycles/h≈0.00≪목표, p80=0.114 → 기회 확대 위해 완화(−8.0%) | — |
| 2025-09-14 HH:MMZ | ETH | `ENTRY_RETEST_WINDOW_S`   | 120  | 150  | 24h cycles/h=0.00, ATR=1.10(p80=0.188, Gate≈0.207%) → 신호 허용시간 확대 | — |
| 2025-09-15T02:29Z | BTC | `ATR_GATE_MULT` | 1.15 | 1.10 | 24h cycles/h=0.00≪목표0.20, p80=0.099 → 소폭 완화(−4.4%) | — |
| 2025-09-15T02:29Z | ETH | `ATR_GATE_MULT` | 1.10 | 1.06 | 24h cycles/h=0.00<0.30, p80=0.155, 쿨다운300·윈도우150 유지 → 소폭 완화(−3.6%) | — |
| 2025-09-16T01:53Z | BTC | `ATR_GATE_MULT`        | 1.10 | 1.02 | 24h cycles/h=0.00≪목표0.20, p80=0.113↑ → 게이트 하한으로 완화(−7.3%) | — |
| 2025-09-16T01:53Z | ETH | `ENTRY_RETEST_TOL_PCT` | 0.08 | 0.09 | cycles/h=0.00<0.30, p80=0.186로 게이트↑ → 리테스트 허용폭 확장(+0.01) | — |
| 2025-09-16 HH:MMZ | BTC | `ATR_GATE_MULT`        | 1.10 | 1.02 | 24h cycles/h=0.00≪목표0.20, p80=0.113 → 게이트 하한으로 완화(−7.3%) | — |
| 2025-09-16 HH:MMZ | ETH | `ENTRY_RETEST_TOL_PCT` | 0.09 | 0.10 | cycles/h=0.00<0.30, p80=0.186↑ → 리테스트 허용폭 확장(+0.01)       | — |
| 2025-09-20T00:00Z | BTC | `ENTRY_RETEST_WINDOW_S` | 90 | 120 | 24h cycles=0 → 매칭폭 확대로 빈도↑ 시도 | — |
| 2025-09-20T00:00Z | ETH | `ATR_GATE_MULT` | 1.06 | 1.02 | p80=0.189 → Gate 0.200%→0.193%(−3.8%), 무진입 완화 | — |
| 2025-09-20T03:42Z | BTC | `ENTRY_RETEST_TOL_PCT`   | unset | 0.09 | 24h cycles=0, PF=0.00, p80=0.127% → 리테스트 통과율 보수적 확대 |
| 2025-09-20T03:42Z | ETH | `ENTRY_RETEST_WINDOW_S`  | 150   | 180  | 24h cycles=0, PF=0.00, p80=0.14%  → 타이밍 미스 완화 |
| 2025-09-22T02:26Z | BTC | `ENTRY_RETEST_WINDOW_S` | 120 | 180 | 24h cycles=0, BTC p80=0.054% → 타이밍 미스 완화 | — |
| 2025-09-22T02:26Z | ETH | `ENTRY_RETEST_TOL_PCT`  | 0.10 | 0.11 | 24h cycles=0, ETH p80=0.099% → 타겟 존 1bp 완화 | — |
| 2025-09-23T05:06Z | BTC | `ENTRY_RETEST_TOL_PCT`  | 0.09  | 0.15 | 24h cycles=0, p80=0.136% → 리테스트 폭 완화 | — |
| 2025-09-23T05:06Z | BTC | `ENTRY_RETEST_WINDOW_S` | 180   | 300  | 리테스트 타이밍 미스 완화                   | — |
| 2025-09-23T05:06Z | BTC | `ENTRY_COOLDOWN_S`      | 600   | 300  | 기회 재포착률↑/수수료 관리 하한선 300s      | — |
| 2025-09-23T05:06Z | ETH | `ENTRY_RETEST_TOL_PCT`  | 0.11  | 0.13 | 24h cycles=0, p80=0.233% → 폭 소폭 완화      | — |
| 2025-09-23T05:06Z | ETH | `ENTRY_RETEST_WINDOW_S` | 180   | 240  | 타이밍 미스 완화                              | — |
| 2025-09-24T01:30Z | BTC | `ENTRY_RETEST_TOL_PCT`  | 0.15 | 0.18 | 24h cycles=0, ATR p80=0.14% → 통과율↑ | — |
| 2025-09-24T01:30Z | BTC | `ENTRY_RETEST_WINDOW_S` | 300  | 420  | 리테스트 타이밍 미스 완화               | — |
| 2025-09-24T01:30Z | BTC | `ENTRY_COOLDOWN_S`      | 300  | 240  | 기회 재포착률↑                           | — |
| 2025-09-24T01:30Z | ETH | `ENTRY_RETEST_TOL_PCT`  | 0.13 | 0.16 | 24h cycles=0, ATR p80=0.207% → 통과율↑  | — |
| 2025-09-24T01:30Z | ETH | `ENTRY_RETEST_WINDOW_S` | 240  | 360  | 리테스트 타이밍 미스 완화               | — |
| 2025-09-24T01:30Z | ETH | `ENTRY_COOLDOWN_S`      | 300  | 240  | 기회 재포착률↑                           | — |
| 2025-09-25T03:41Z | BTC | `ENTRY_RETEST_TOL_PCT`  | 0.18 | 0.22 | 24h cycles=0, ATR p80=0.134% → 통과율↑ | — |
| 2025-09-25T03:41Z | BTC | `ENTRY_RETEST_WINDOW_S` | 420  | 600  | 리테스트 타이밍 미스 완화                 | — |
| 2025-09-25T03:41Z | BTC | `ENTRY_COOLDOWN_S`      | 240  | 120  | 재기회 포착률↑                             | — |
| 2025-09-25T03:41Z | ETH | `ENTRY_RETEST_TOL_PCT`  | 0.16 | 0.20 | 24h cycles=0, ATR p80=0.198% → 통과율↑   | — |
| 2025-09-25T03:41Z | ETH | `ENTRY_RETEST_WINDOW_S` | 360  | 480  | 리테스트 타이밍 미스 완화                 | — |
| 2025-09-25T03:41Z | ETH | `ENTRY_COOLDOWN_S`      | 240  | 120  | 재기회 포착률↑                             | — |
| 2025-09-26T06:03Z | BTC | `TIMEFRAME`              | 5m?  | 1m   | 신호 빈도↑ (Al Brooks/ICT 실행 타임프레임 참고) | — |
| 2025-09-26T06:03Z | BTC | `ENTRY_RETEST_TOL_PCT`   | 0.22 | 0.30 | 24h ATR p80=0.167% 상회, 구간 리테스트 허용     | — |
| 2025-09-26T06:03Z | BTC | `ENTRY_RETEST_WINDOW_S`  | 600  | 900  | 되돌림 타이밍 미스 완화                           | — |
| 2025-09-26T06:03Z | BTC | `ENTRY_COOLDOWN_S`       | 120  | 60   | 재기회 포착률↑                                    | — |
| 2025-09-26T06:03Z | ETH | `TIMEFRAME`              | 3m?  | 1m   | 신호 빈도↑ (TraderSZ/ICT 실행 타임프레임 참고)    | — |
| 2025-09-26T06:03Z | ETH | `ENTRY_RETEST_TOL_PCT`   | 0.20 | 0.35 | 24h ATR p80=0.324% 근접/상회                       | — |
| 2025-09-26T06:03Z | ETH | `ENTRY_RETEST_WINDOW_S`  | 480  | 900  | 되돌림 타이밍 미스 완화                           | — |
| 2025-09-26T06:03Z | ETH | `ENTRY_COOLDOWN_S`       | 120  | 60   | 재기회 포착률↑                                    | — |
| $(date -u +%Y-%m-%dT%H:%MZ) | BTC | `TIMEFRAME` | 1m | 5m | 신호미유입→코드 기본 TF 정렬, 파이프라인 복구 | — |
| $(date -u +%Y-%m-%dT%H:%MZ) | ETH | `TIMEFRAME` | 1m | 3m | 신호미유입→코드 기본 TF 정렬, 파이프라인 복구 | — |
| When (UTC) | Symbol | Param | Before | After | Rationale | Notes |
| 2025-09-29T04:18Z | BTC | ATR_GATE_MULT | 1.02 | 0.918 | 24h cycles/h=0.00, PF=0.00 → 진입 완화(-10%) | p80=0.088% → gate thr 0.0898%→0.0808% |
| 2025-09-29T04:18Z | ETH | ATR_GATE_MULT | 1.00 | 0.90 | 24h cycles/h=0.00, PF=0.00 → 진입 완화(-10%) | p80=0.140% → gate thr 0.1400%→0.1260% |
| 2025-09-30T07:35Z | BTC | ENTRY_RETEST_TOL_PCT | 0.30 | 0.45 | 24h cycles/h=0.00, PF=0.00 → 진입 완화(리테스트 허용오차 확대) | ATR gate 1.02 유지 |
| 2025-09-30T07:35Z | ETH | TIMEFRAME | 1m | 3m | 24h cycles/h=0.00, PF=0.00 → ATR/신호 타임프레임 정합성 확보 | ATR gate 1.00 유지 |
| 2025-10-01T05:06Z | BTC    | `ATR_GATE_MULT`  | 1.02   | 0.96  | 24h cycles/h=0.00, PF=0.00 → 게이트 완화로 기회 확대        | TF 5m 유지  |
| 2025-10-01T05:06Z | ETH    | `ATR_GATE_MULT`  | 1.00   | 0.95  | 24h cycles/h=0.00, PF=0.00 → 게이트 완화로 기회 확대        | TF 3m 유지  |

