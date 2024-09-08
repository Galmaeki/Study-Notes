- 코드를 아래처럼 구성해도 빈 생성에 오류가 발생함
```java
@SpringBootApplication(
        scanBasePackages = {
                "dev.be.moduleapi",
                "dev.be.modulecommon"
        }
)
public class ModuleApiApplication {

    public static void main(String[] args) {
        SpringApplication.run(ModuleApiApplication.class, args);
    }

}
```
- 프로젝트 구성에는 Entity 와 Repository 가 common 모듈에 구성되어 있음

#### @SpringBootApplication 으로 베이스 패키지를 스캔하면 되는게 아닌가?
- 아님
- scanBasePackages 에 모듈 경로를 지정하여 엔티티나 레포지토리도 함께 스캔 할 수 있을것으로 예상했음


- SpringBootApplication 은 스프링 빈만 스캔하는 어노테이션
  - Spring Bean 에 @Entity 는 포함되지 않으니 스캔되지 않음
  - @EntityScan 어노테이션을 사용하여 entity 가 존재하는 모듈도 스캔해야 함
- SpringBootApplication 은 빈을 **등록**하는 어노테이션
  - Jpa 레포지토리는 스프링 컴포넌트 스캔의 대상이 아닌 JPA 가 관리하는 객체
  - Component Scan 만으로는 Jpa 가 제공하는 기능을 사용 할 수 없음
  - @EnableJpaRepositories 어노테이션을 사용하여 Jpa 가 JPA 레포지토리를 스캔 할 수 있도록 설정 해야 함

### 결론
```java
@SpringBootApplication(
        scanBasePackages = {
                "dev.be.moduleapi",
                "dev.be.modulecommon"
        }
)
@EntityScan("dev.be.modulecommon.entity")
@EnableJpaRepositories("dev.be.modulecommon.repository")
public class ModuleApiApplication {

    public static void main(String[] args) {
        SpringApplication.run(ModuleApiApplication.class, args);
    }

}
```
- 적절한 어노테이션을 사용하여 경로를 지정해 주었음