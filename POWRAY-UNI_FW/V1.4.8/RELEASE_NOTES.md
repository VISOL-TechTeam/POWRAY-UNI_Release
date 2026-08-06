# POWRAY-UNI V1.4.8 (2026-08-06)

WIFI_MODULE 전용 POWRAY 상태요청(A9) 응답 개선. DMX / W_DMX / RS485 경로는 변경 없음.

- WiFi Convert: A9는 Duty_Adjust_End 중에도 수신 (CONT만 duty 게이트)
- WIFI 전용 명령 큐 + ProtocolTask 우선 처리
- WifiTask / ProtocolTask notify로 폴링 지연 감소
- Send_Status: huart1 IT TX (huart2 블로킹 유지)

실기: ESP MQTT Broadcast A5↔A9 스트레스로 /Data 24B 대응률·RTT 확인 권장.
