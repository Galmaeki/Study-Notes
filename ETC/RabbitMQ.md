## RabbitMQ
- AMQP를 구현한 오픈소스 메시지 브로커
- Producer 에서 Consumer 로 메시지를 전달하는 Broker 역할을 함

### 중요 개념
- Producer : 요청을 보내는 주체로 보내고자 하는 메시지를 Exchange 에 Publish 함
- Consumer : Producer 로 부터 메시지를 받아 처리하는 주체
- Exchange : Producer 로 부터 받은 메시지를 어떤 Queue 로 보낼 지 결정함, 4가지 타입이 존재
- Queue : Consumer 가 메시지를 소비하기 까지 보관하는 저장소
- Binding : Exchange 와 Queue 의 관계, 보통 사용자가 Exchange 가 특정 Queue 를 Binding 하도록 정의(Fanout 은 예외)

#### Exchange 속성
- Name : Exchange 이름
- Type : 메시지 전달 방식 - AMQP 의 교환타입
  - Direct
  - Fanout
  - Topic
  - Headers
- Durability : 브로커가 재시작 될 때 남아있는지 여부
  - Durable : 브로커가 재시작 되어도 디스크에 저장되어 남아있음
  - Transient : 브로커가 재시작되면 사라짐
- Auto-delete : 마지막 Queue 연결이 해제되면 삭제

#### Queue 속성
- Name : Queue 이름 - amq. 은 예약어로 사용 불가
- Durability : 브로커가 재시작 될 때 남아있는지 여부
  - Durable : 브로커가 재시작 되어도 디스크에 저장되어 남아있음
  - Transient : 브로커가 재시작 되면 사라짐
- Auto delete : 마지막 Consumer 가 소비를 끝낼 경우 자동 삭제
- Argument : 메시지 TTL, Max Length 같은 추가 기능 명시

### Kafka 와 비교
- 방식의 차이
  - Kafka : Pub/Sub
    - 생산자 중심의 설계로 구성
    - 생산자가 원하는 각 메시지를 게시 할 수 있도록 하는 메시지 배포 패턴으로 진행
  - RabbitMQ : 메시지 브로커
    - 브로커 중심적 설계로 구성
    - 지정된 수신인에게 메시지를 확인, 라우팅, 저장 및 배달 하는 역할을 수행하여 보장되는 메시지 전달에 중점
- 메시지의 휘발성
  - Kafka
    - 메시지를 Topic 으로 분류하여 이를 Event Streamer 에 저장
    - 수신자가 특정 Topic 에 대한 메시지를 가져가도 Event Streamer 는 Topic 을 유지하여 문제가 발생해도 재생 가능
  - RabbitMQ
    - Queue 에 저장된 메시지에 대해 Event Consumer 가 메시지를 가져가면 Queue 에서 해당 메시지 삭제
- 용도
  - Kafka
    - 실시간 로그 수집
    - 이벤트 스트리밍
    - 데이터 파이프라인 구축
    - 대용량 데이터의 처리와 분석
  - RabbitMQ
    - MSA 간 비동기 통신
    - 작업 큐
    - 다양한 메시징 패턴(요청/응답 라우팅 등)

### 설치
1. Erlang 설치
   - Erlang 런타임에서 동작하므로 Erlang를 먼저 설치 해야함
2. Erlang 환경 변수 등록
3. Rabbit MQ 공식 홈페이지에서 설치파일 다운 및 실행
4. RabbitMQ 설치 폴더의 sbin 을 환경변수 등록
5. Management Plugin 설치
   - 공식 문서에서 설치를 권장함
```shell
rabbitmq-plugins enable rabbitmq_management
```
6. localhost:15672 에 접근하여 로그인
   - Username & Password : guest
