# 24-7-coinbot-docs
개인 연구용 코인 자동매매(알고) 봇 문서 – 설계/백테스트/운영 노트 (투자권유 아님)   
Docs for a personal crypto trading bot — design/backtests/ops (not financial advice)

개인 연구용 **코인 자동매매/분석 프레임워크** 문서 레포입니다.  
저는 이 시스템을 **개인 계정에서 소규모로 직접 테스트/운용**해 보았습니다.

돈많은 백수를 꿈꿔 24시간 자동 코인매매봇 프로토타입 만들게 되었습니다.

오라클 무료 가상 인터넷 환경을 이용해 24시간 돌아가는 코인봇과 바이낸스 계정 key와 연결하여 실매매가 되는 코인봇을 만듦

> **면책(Disclaimer)**  
> 본 문서는 **교육/연구 목적**으로만 제공되며 **투자 조언/권유가 아닙니다**.  
> 본 문서를 따라 하거나 코드를 사용하여 발생하는 **모든 손실은 사용자 본인 책임**입니다.  
> 거래소 약관 및 관할 법규를 준수해 주세요.

## What’s inside
- 아키텍처 개요(전략, 리스크, 실행/체크 루프) ['docs/infra/architecture']('docs/infra/architecture')
- 파라미터 변경 로그/파라미터 변경 이유 및 값 
- 생성한 커멘드 

## Quick Links
- **Docs Home**: `./docs/index.md`
- **Issues**: [New Issue](../../issues/new/choose)

## License
- **Docs**: CC BY 4.0 (see LICENSE)
- Parameter changes: [`ops/param-log.md`](ops/param-log.md)  
- Daily run logs: [`ops/run-logs/`](ops/run-logs/)
- ## Infrastructure & 24×7 Ops
- Cloud VM에서 24시간 운영하는 방법: [docs/infra/24x7-vm.md](docs/infra/24x7-vm.md)


