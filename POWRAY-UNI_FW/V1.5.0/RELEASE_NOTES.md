# POWRAY-UNI V1.5.0 (2026-09-04)

WIFI 모드에서 USART2 RS485 POWRAY 병행.

- COMM=WIFI 여도 USART2 수신 프레임을 ProtocolTask가 처리
- 응답은 수신 포트로 송신 (WIFI=USART1, RS485=USART2)
- WIFI 명령은 별도 버퍼로 처리해 USART2 수신 버퍼를 덮어쓰지 않음
- USART2 UART 에러 후에도 수신을 재등록

## Patch 2026-09-09

부스트 동작 시간 제한.

- STATUS.Boost는 디바이스 부스트 최대 동작 초. 통신 디밍 100%와는 별개
- 통신 부스트(밝기 > 100) 요청 초가 설정값보다 크면 설정값만큼만 동작
- 설정값 0은 부스트 기능 사용 안 함(통신/수동 스위치 모두 부스트 시작 안 함)
