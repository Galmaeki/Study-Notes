## UDP
- User Datagram Protocol

### 특징
- 비연결성
  - 송신자, 수신자간 연결을 설정하지 않음
    - 연결설정과정 없이 데이터가 전송되어, 지연시간이 적고 빠름
    - 손실을 알 수 없고 순서가 바뀔 수 있음
- DNS 나 실시간 스트리밍, 온라인 게임 등에서 사용
- 양방향 통신 가능
- 체크섬을 통해 오류 감지

### IP 통신과의 비교
- IP 통신에는 Port 가 없어 어떤 프로세스로 데이터를 전달 할 지 알 수 없음