## JPA 란?
- Java 에서 ORM 기술 표준으로 사용되는 인터페이스의 모음
- 자바에서 관계형 DB를 사용하는 방식을 정의한 인터페이스
- 이를 구현한 대표적 구현체로 Hibernate, OpenJPA 등이 존재함

### JPQL
- Java Persistence Query Language
- 객체를 대상으로 쿼리를 작성하는 언어
- SQL 과 유사하나, DB가 아닌 엔티티를 기반으로 작성하여 DB 종속적이지 않음

### 영속성 컨텍스트
- JPA 에서 엔티티를 관리하는 일종의 캐시로 비유 할 수 있음
- 애플리케이션과 DB 사이에서 엔티티의 상태를 추적하고 환리하여 데이터의 일관성과 무결성을 보장