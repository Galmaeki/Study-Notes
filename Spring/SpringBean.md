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

### Bean Scope(빈 스코프)
- 빈이 존재할 수 있는 범위를 말함
- 빈은 기본적으로는 싱글톤 스코프로 스프링 컨테이너가 시작될 때 함께 생성되어 컨테이너가 종료될 때 종료됨
- 종류
  - 싱글톤 : 기본 스코프, 컨테이너의 사작과 종료까지 유지됨
  - 프로토타입 : 빈 요청시마다 새로운 인스턴스가 생성됨, 빈의 초기화와 의존성 주입까지만 관리하고 이후는 관리하지 않아 빈을 사용하는 쪽에서 나머지 관리를 해야함
  - 리퀘스트 : Http 요청마다 새로 인스턴스가 생성됨
  - 세션 : Http 세션마다 빈 인스턴스가 생성됨
  - 애플리케이션 : 서블릿 컨텍스트당 하나의 인스턴스가 생성되어 애플리케이션 전체에서 공유됨

### Bean LifeCycle(빈 생명주기)
- 빈이 생성되고 사용되며 소멸되는 전체 과정
1. 스프링 컨테이너 생성
2. 빈 생성
3. 의존성 주입
4. 초기화 콜백
5. 빈 사용
6. 소멸 전 콜백
7. 스프링 종료