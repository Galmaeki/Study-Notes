## Jpa Projection
- Jpa에서 전체 속성이 아닌 특정 속성만 조회가 필요할 때 사용하는 방식
- [Projection 공식문서](https://docs.spring.io/spring-data/jpa/reference/repositories/projections.html#projections.interfaces)

#### Projection
- 테이블에서 출력하고자 하는 열을 제한적으로 출력하는 기능
### 기본 개념
- Jpa에서 객체 조회시, 기본적으로 전체 속성을 조회하나(Aggregate Root 객체 사용), 일부 기능만 필요할 때 사용함

### 종류
#### Sample
- 설명을 위한 Person 샘플
```java
class Person {
    @Id UUID id;
    String firstname, lastname;
    Address address;

    static class Address {
        String zipCode, city, street;
    }
}

interface PersonRepository extends Repository<Person, UUID> {
    Collection<Person> findByLastname(String lastname);
}
```

## 인터페이스 기반 프로젝션(Interface-based Projections)
- 필요 속성에 대한 Getter 메소드를 포함한 인터페이스를 정의하고 반환타입으로 사용함
- 이 때 실제로 Person의 일부 데이터만 조회함
- 실행시 인터페이스의 프록시를 생성하고, 메소드 호출을 Person 객체의 해당 속성에 위임
- **인터페이스의 메소드가 엔티티의 프로퍼티 이름&구조와 매칭되어야 조회 가능**
```java
interface NamesOnly{
    String getFirstname();
    String getLastName();
}
```
```java
interface PersonRepository extends Repository<Person, UUID> {
    Collection<NamesOnly> findByLastname(String lastname);
}
```
---
- 재귀적 프로젝션으로 엔티티 내부에 포함된 다른 객체도 적용 가능함
```java
interface PersonSummary{
    String getFirstname();
    String getLastName();
    AddressSummary getAddress();
    
    interface AddressSummary{
        String getCity();
    }
}
```

### Closed Projections
- 프로젝션의 인터페이스 메소드가 모두 엔티티의 실제 속성과 1:1 매핑되는 형태
- 샘플코드의 NamesOnly와 같은 형태
- 필요한 속성만 쿼리를 보내기 때문에 불필요한 데이터를 조회하지 않음

### Open Projections
```java
interface NamesOnly {
    @Value("#{target.firstname + ' ' + target.lastname}")
    String getFullName();
}
```
- @Value 어노테이션으로 SpEL로 계산된 값을 노출
- 값을 조합하여 새로운 값으로 리턴함
- Spring Data JPA 의 쿼리최적화가 적용되지 않을 수 있음
- @Value를 너무 복잡하게 구성하면 안됨
- 간단한 표현식의 경우 Java 8 의 default 메소드를 활용
```java
interface NamesOnly {

  String getFirstname();
  String getLastname();

  default String getFullName() {
    return getFirstname().concat(" ").concat(getLastname());
  }
}
```
- 이와 같은 식으로 구성할 땐 다른 접근 제어자 메소드 만으로 이를 구현할 수 있어야함
#### Nullable Wrapper
- 프로젝션의 getter는 Null 래퍼를 허용하여 타입 안정성을 추가할 수 있음
```java
interface NamesOnly{
    
    Optional<String> getFirstname();
}
```

## 클래스(DTO) 기반 프로젝션(Class-based Projections)
- 인터페이스 대신 DTO 로 반환받는 방식
- 일부 필드만 로딩 할 경우, 노출된 파라미터 이름과 엔티티 필드명을 기준으로 판단함
- 프록시를 생성하지 않음
- 중첩 프로젝션 적용 불가
- DTO 프로젝션은 생성자 1개를 권장함
  - 여러개인 경우 @PersistenceCreator 을 사용하여 지정해야함
```java
interface PersonRepository extends Repository<Person, UUID> {

  <T> Collection<T> findByLastname(String lastname, Class<T> type);
}
```
```java
void someMethod(PersonRepository people) {

  Collection<Person> aggregates =
    people.findByLastname("Matthews", Person.class);

  Collection<NamesOnly> aggregates =
    people.findByLastname("Matthews", NamesOnly.class);
}
```

## 동적 프로젝션(Dynamic Projections)
- 호출 시점에 프로젝션을 결정하는 방식
- 호출 시 type에 지정되는 클래스에 따라 적용된 프로젝션으로 받게됨
```java
<T> List<T> findByLastname(String lastname, Class<T> type);
```