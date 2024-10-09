## ORM
- Object-Relational-Mapping
- 객체 지향 언어와 DB 테이블을 매핑하는 기술들을 말함

### 장점
- SQL 문을 사용하지 않고 DB를 조작하여 개발자는 비즈니스 로직 구성에 집중 가능
- 보일러 플레이트가 줄어듬
- 객체 지향적 코드 작성 가능
- DB를 변경해도 쿼리를 수정 할 필요가 없음

### 단점
- 프로젝트 규모가 크고 설계가 복잡한 경우, 속도 저하 및 일관성을 무너뜨리는 문제가 발생 가능
- 복잡하고 무거운 쿼리는 별도의 튜닝이 필요

### 종류
- Java : Hibernate, EclipseLink 등
- Python : SQLAlchemy 등
- Java Script : TypeORM, Sequelize 등