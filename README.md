# 🚀 Release Notes

## [v1.4.1] - 2026-04-30
### ✨ Added
 **W-DMX RX-only 부트로더 업로드 및 백업 펌웨어 복구 지원 추가**
- Flash sector 4-7을 APP 영역(0x08010000-0x0807FFFF)으로 축소
- Flash sector 8-11을 백업 펌웨어 영역(0x08080000-0x080FFFFF)으로 분리
- APP vector table 뒤 고정 메타 영역에 magic/size/crc/version 정보 삽입
- 업로더가 APP 이미지 마지막 8byte에 동일 magic을 추가하여 업로드 중단/부분 손상 검출
- 부트로더가 MSP/Reset vector, 앞뒤 magic, 이미지 size, CRC를 모두 확인 후 APP 실행
- RS485 업로드 성공 시 검증 완료된 APP만 백업 영역으로 복사
- W-DMX 250000 bps 8N2 RX-only 업로드 경로 추가
- W-DMX는 TX 응답이 없으므로 START/WRITE/COMPLETE 프레임 반복 송신 방식 적용
- W-DMX 업로드 실패 시 유효한 백업 펌웨어가 있으면 APP 영역으로 복구 후 리셋
- W-DMX 업로드 성공/실패/백업복구 결과를 부트로더 OLED 화면에 표시 후 리셋
- 부트로더 WAITING UPLOAD 화면 우측 하단에 부트로더 버전 표시
- APP 동작 중 W-DMX 수신 경로에서도 부트로더 업로드 요청 패턴 감지
- CMake 산출물 파일명은 FW_VERSION_FILE_NAME 기준으로 생성
- 업로더는 파일명 내 V1_4_1, V1.4.1 형식의 펌웨어 버전을 자동 파싱
- RS485 부트로더 CMD_DISCOVER 수신 불가 문제 해결
- 부팅 시 RS485 DE 핀이 TX 고정 → BL_PROTOCOL_InitRxMode()로 RX 모드 강제 초기화
- BL_DMX_Poll이 USART1 데이터 없어도 80ms 블로킹 → USART2 CMD_DISCOVER overrun 손실
    main loop에서 USART1 RXNE/FE/ORE 플래그 없으면 BL_DMX_Poll 호출 생략으로 해결
- CMD_DISCOVER 응답 delay 연산자 우선순위 버그 수정 (id+1)*15 → (id+1)*15ms
- WaitForReady 내 CMD_DISCOVER 즉시 처리 추가 (CMD_READY 대기 중 무시되던 문제 해결)
     
**업로더(POWRAY-UNI Uploader) V1.0 업데이트**
  - RS485 echo로 인한 except break 조기 종료 버그 수정 (break → continue)
  - 수동 ID 추가 기능 추가 (QSpinBox + 수동 추가 버튼)
  - 포트 오픈 후 1초 안정화 대기 및 버퍼 초기화 로직 추가

## [v1.4.0] - 2026-04-30
### ✨ Added
 **RS485 부트로더 업로드 지원 추가**
- APP 동작 중 9600 bps 32바이트 패턴으로 업로드 모드 요청 감지
- FRAM 업로드 요청 플래그 기록 후 리셋하여 부트로더 진입
- 부트로더 업로드 통신은 921600 bps로 수행
 
 **RS485 Uploader V0.1 추가**
  1. HEX 파일 App 펌웨어 업로드 기능 수행
  2. RS485 통신으로 업로드 모드 진입 및 업로드 취소 기능
  3. App 동작시 9600bps, 업로드시 921600bps 적용


## [v1.3.2] - 2026-04-29
### ✨ Added
 **W-DMX Receiver 상태 신호 표시 기능 추가**
- W-DMX 모드에서 PA11(NRST_IO)을 Receiver status 입력 핀으로 전환
- 0V 지속, 100 ms/100 ms 점멸, 100 ms/900 ms 점멸, 3.3V 지속 패턴 판별
- Status_Display1 COMM 항목에 W_DMX([X]/[~]/[O]/[G]) 형식으로 상태 표시
- W-DMX 모드에서는 WiFi NRST 제어가 PA11 상태 입력을 건드리지 않도록 보호
- W-DMX 해제 시 PA11을 WiFi NRST 출력 용도로 복구

## [v1.3.1] - 2026-04-28
### ✨ Added
 **유선 DMX 수신 방식을 W-DMX와 동일한 프레임 누적/Break 동기화 구조로 개선**
- USART2 DMX 모드는 250 kbps, 2 Stop-bits 설정에서만 동작하도록 COMM == DMX 조건 강화
- COMM이 DMX가 아닐 경우 USART2를 일반 RS485 설정(9600 bps, 1 Stop-bit)으로 복귀
- 유선 DMX는 슬롯 간 IDLE 오검출로 버퍼가 초기화되지 않도록 FE(Break) 기준으로 프레임 처리
- DMX 수신 데이터는 다음 Break 전까지 RS485.Rxd_buf[]에 누적 후 이전 프레임을 처리
- DMX/W-DMX 모두 Start Code 0x00 및 My_ID 채널 기준으로 Dimming/Duty 갱신

## [v1.3.0] - 2026-04-27
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
 
## [v1.2.0] - 2026-03-17
### ✨ Added
- **신규 모델 지원**: `2.0KW_J` (200V / 110V 겸용) 모델 대응 추가

## [v1.1.1] - 2025-12-05
### ✨ Added
- **통신 프로토콜**: WIFI Module Protocol 연동 기능 추가

## [v1.0.0]
### 🌱 Initial Release
- 시스템 기본 기능 구현 및 초기 배포
