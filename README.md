# 🚀 Release Notes

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