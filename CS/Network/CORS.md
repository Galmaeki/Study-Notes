## CORS
- Cross Origin Resource Sharing
- 같은 출처에서만 자원을 공유 할 수 있도록 하는 브라우저 정책
  - 브라우저 정책이므로 서버 to 서버 통신에서는 적용되지 않음
- CSRF, CSS 공격에 취약한 부분을 해결하기 위해 나옴
- 프로토콜, 도메인, 포트까지 일치해야 같은 출처로 인정됨

#### Preflight Request
- 요청이 유효한 요청인지 먼저 확인하는 요청
- Option 메소드를 사용
- 단순 요청(GET, POST, HEAD)시에는 보내지 않음
- PUT, DELETE 메소드를 사용하거나 커스텀 헤더를 사용하는 경우에 보내짐

#### CORS Header
- 서버는 클라이언트에게 리소스 접근 허용을 위해 응답 헤더에 CORS 관련 헤더를 포함해야함
  - Access-Control-Allow-Origin : 허용되는 출처
  - Access-Control-Allow-Methods : 허용되는 메소드
  - Access-Control-Allow-Headers : 허용되는 헤더
  - Access-Control-Allow-Credentials : 자격 증명이 포함된 요청 허용 여부