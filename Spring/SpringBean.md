## Spring Bean
- 스프링 컨테이너에 의해 관리되는 재사용 가능한 소프트웨어 컴포넌트
- 인스턴스화 된 객체를 의미함

### 사용하는 이유
- 스프링 간 객체가 의존관계를 관리하도록 하는것이 가장 큰 목적
- 객체가 의존관계를 등록 할 때 스프링 컨테이너에서 해당하는 빈을 찾고 그 빈과 의존성을 만듬

### 스프링 빈 등록 방법
- xml에 직접 등록
- @Bean 어노테이션 사용
- @Component, @Controller, @Service, @Repository 어노테이션을 이용

#### XML에 직접 등록
- \<bean> 태그를 사용
```xml
<bean id="helloService" class="com.example.myapp.di.HelloService"/>

<bean id="helloController" class="com.example.myapp.di.HelloController" p:helloService-ref="helloService">
</bean>
```
1. application-config.xml을 resource 하위에 생성
2. \<beans> 태그 안에 \<bean> 태그를 사용
- 위 코드는 의존성 주입까지 진행된 코드

#### @Bean 어노테이션
- 인스턴스를 반환하고 빈에 등록하는 코드
- 메소드 위에 @Bean 태그를 사용하고 클래스 위에 @Configuration 을 사용함
```kotlin
@Configuration
class AppConfig {

    @Bean
    fun myService(): MyService {
        return MyServiceImpl()
    }
}
```

#### @Component, @Controller, @Service, @Repository 어노테이션을 이용
```kotlin
@Controller
class MyController(
    private val myService: MyService
) {
    //Do Something...
}
```

---
### @Bean 과 @Component 차이
- @Component 는 클래스에 붙여 해당 클래스가 빈으로 등록 되도록 자동으로 처리
  - 주로 컴포넌트 스캔을 통해 자동으로 빈을 생성, 등록
- @Bean 은 메소드에 사용하여 메소드의 반환값을 스프링 컨테이너에 빈으로 등록
  - 개발자가 빈의 생성 과정을 제어해야 할 때 사용

# Todo...
- SpringBean LifeCycle
  - @PostConstruct, @PreDestroy
- @Configuration
- Spring Bean Scope
  - 기본적 싱글톤이나, 필요에 따라 프로토타입, 세션등으로 변경 가능
- Lazy Initialization
  - @Lazy
