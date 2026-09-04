# POWRAY-UNI V1.5.0 (2026-09-04)

WIFI 모드에서 USART2 RS485 POWRAY 병행.

- COMM=WIFI 여도 USART2 수신 프레임을 ProtocolTask가 처리
- 응답은 수신 포트로 송신 (WIFI=USART1, RS485=USART2)
- WIFI 명령은 별도 버퍼로 처리해 USART2 수신 버퍼를 덮어쓰지 않음
- USART2 UART 에러 후에도 수신을 재등록
