## Transaction
- DB 에서 여러 작업이 하나의 논리 단위로 묶여 실행되는 것
- 무결성과 일관성을 유지하기 위함

### ACID
- 원자성(Atomicity)
  - 하나의 트랜잭션은 모두 성공하거나 모두 실패해야 함
- 일관성(Consistency)
  - 트랜잭션 완료 후 DB의 제약 조건이 만족되어야 함
- 고립성(Isolation)
  - 각 트랜잭션은 다른 트랜잭션으로 부터 독립적으로 실행 되어야 함
- 지속성(Durability)
  - 트랜잭션이 성공하면 결과는 영구적으로 저장되어야 함

---

### 트랜잭션 격리 수준
- 각 트랜잭션이 서로 간섭하지 않도록 보장하는 정도를 설정
1. Read Uncommitted
2. Read Committed
3. Repeatable read
4. Serializable

#### Read Uncommitted
- 각 트랜잭션에서의 변경 내용을 커밋이나 롤백 여부 상관없이 읽어 올 수 있음
- Dirty Read 현상 발생
  - 트랜잭션이 끝나지 않아도 다른 트랜잭션에서 조회 될 수 있음

#### Read Committed
- 실제 테이블 값이 아닌 Undo 영역에 백업된 레코드에서 값을 가져옴
- Non-repeatable Read 발생
  - 한 트랜잭션에서 같은 쿼리 두 번 실행시 값이 이전과 다르게 나타나는 현상

#### Repeatable Read
- Undo 공간에 백업해두고 실제 레코드 값을 변경
- MySQL 에서는 트랜잭션마다 트랜잭션 Id를 부여하여 트랜잭션 ID 보다 작은 번호에서 변경된 값만 조회
- Phantom Read 발생
  - 다른 트랜잭션에서 수행한 변경 작업으로 조회되는 데이터의 수가 이전과 다르게 조회되는 현상

#### Serializable
- 다른 사용자는 트랜잭션 영역에 해당되는 데이터를 수정하거나 입력 할 수 없음
- 이상현상은 발생하지 않으나 동시처리 성능이 낮음
