# 🚀 Release Notes

## [v1.32] - 2026-04-29
### ✨ Added
 **W-DMX Receiver 상태 신호 표시 기능 추가**
- W-DMX 모드에서 PA11(NRST_IO)을 Receiver status 입력 핀으로 전환
- 0V 지속, 100 ms/100 ms 점멸, 100 ms/900 ms 점멸, 3.3V 지속 패턴 판별
- Status_Display1 COMM 항목에 W_DMX([X]/[~]/[R]/[D]) 형식으로 상태 표시
- W-DMX 모드에서는 WiFi NRST 제어가 PA11 상태 입력을 건드리지 않도록 보호
- W-DMX 해제 시 PA11을 WiFi NRST 출력 용도로 복구

## [v1.31] - 2026-04-28
### ✨ Added
 **유선 DMX 수신 방식을 W-DMX와 동일한 프레임 누적/Break 동기화 구조로 개선**
- USART2 DMX 모드는 250 kbps, 2 Stop-bits 설정에서만 동작하도록 COMM == DMX 조건 강화
- COMM이 DMX가 아닐 경우 USART2를 일반 RS485 설정(9600 bps, 1 Stop-bit)으로 복귀
- 유선 DMX는 슬롯 간 IDLE 오검출로 버퍼가 초기화되지 않도록 FE(Break) 기준으로 프레임 처리
- DMX 수신 데이터는 다음 Break 전까지 RS485.Rxd_buf[]에 누적 후 이전 프레임을 처리
- DMX/W-DMX 모두 Start Code 0x00 및 My_ID 채널 기준으로 Dimming/Duty 갱신

## [v1.30] - 2026-04-27
### ✨ Added
 **W-DMX (LumenRadio CRMX Pluggy RX) 수신 지원 추가**
 - USART1 을 DMX512 표준(250 kbps, 2 Stop-bits)으로 재초기화 (USART1_WDMX_Init)
 - IDLE Line 인터럽트로 프레임 끝 감지 → WDMX_BreakCallback 호출
   (break_condition 대신 IDLE / FE 이중 감지로 Break 동기화 안정성 향상)
 - WDMX_BreakCallback: 이전 프레임의 채널 데이터를 처리(Dimming/Duty 갱신)
   후 카운터 리셋 → 새 프레임 수신 준비
 - WDMX_RxCallback: 수신 바이트를 wdmx_frame[] 에 순서대로 누적
   (채널 인덱스 = My_ID + 1, 유선 DMX RS485.Rxd_buf 와 동일 방식)
 - 메뉴 시스템에 W-DMX 통신 모드 항목 추가 (OLED Display 연동)
 
## [v1.20] - 2026-03-17
### ✨ Added
- **신규 모델 지원**: `2.0KW_J` (200V / 110V 겸용) 모델 대응 추가

## [v1.11] - 2025-12-05
### ✨ Added
- **통신 프로토콜**: WIFI Module Protocol 연동 기능 추가

## [v1.00]
### 🌱 Initial Release
- 시스템 기본 기능 구현 및 초기 배포