## API Gateway
- API Gateway 패턴은 MSA 의 관리 / 운영을 위한 플랫폼 패턴이며 이 패턴에 필요한 기능들을 제공하는 서버를 말함
- 개별 서비스의 앞 단에서 모든 서비스들의 엔드포인트를 단일화하고 다음과 같은 필수 기능 요소들을 제공

### 역할
- API 요청 로드밸런싱 및 라우팅
- 인증 및 권한 부여
- 서비스 검색 통합
- 응답 캐싱
- 정책, 회로 차단기 및 QoS 다시 시도
- 속도 제한
- 부하 분산
- 로깅, 추적, 상관 관계
- 헤더, 쿼리 문자열 및 청구 변환
- IP 허용 목록에 추가

### 장단점
- 장점
  - 애플리케이션의 내부 구조를 캡슐화
  - 클라이언트와 애플리케이션 간의 왕복 횟수가 감소하여 클라이언트 코드 단순화
- 단점
  - 관리 포인트의 증가

## Spring Cloud API Gateway
- Spring 에서 제공하는 오픈소스 기반 Gateway 서비스
- 요청에 대한 라우팅 기능을 제공
- 응답이 각 서비스로 전달되기 전,후에 처리 가능한 체인필터를 제공
- 로드밸런싱 기능 수행

## 시작하기
1. 프로젝트 생성시 Gateway 의존성 추가
2. Predicate를 yml에 추가(자바 코드로도 가능)
```yaml
server:
  port: 8000

spring:
  application:
    name: gateway-service
  cloud:
    gateway:
      routes:
        - id: first-service
          uri: http://localhost:8081/
          predicates:
            - Path=/first-service/**
        - id: second-service
          uri: http://localhost:8082/
          predicates:
            - Path=/second-service/**

eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone : http://localhost:8761/eureka
```
  - routes : 게이트웨이의 라우트 설정
    - id : 라우트의 식별자
    - uri : 요청이 매치되면 실제로 요청을 전달할 서비스
    - predicates : 요청에 대한 조건 
3. 실행
  - 이 때 실행은 Tomcat가 아닌 Netty 로 실행됨
4. First-service 와 Second-service를 8081, 8082 로 열어둔채로 8000 포트의 /first-service/welcome 주소로 들어가면 접속 가능

### 동작 과정
![springcloud-gateway-01.png](..%2F..%2Fimg%2Fspringcloud-gateway-01.png)
1. Gateway Client 에서 요청을 보냄
2. Handler Mapping에서 요청이 해당 경로(Route)와 일치하면 WebHandler 로 전송
3. WebHandler 에서 요청과 관련된 필터로 전송
4. preFilter 실행
5. 프록시 요청이 실행됨
6. postFilter가 실행됨
#### PreFilter
- 사전 필터
- 클라이언트의 요청이 다른 서버에 전달되기 전 실행되는 필터
#### PostFilter
- 사후 필터
- 서버에서 요청이 돌아온 후 클라이언트에게 전달되기 전에 실행되는 필터

## Predicate 설정
- 두 가지 방법이 존재함
- yml 설정
```yaml
spring:
  application:
    name: gateway-service
  cloud:
    gateway:
      routes:
        - id: first-service
          uri: http://localhost:8081/
          predicates:
            - Path=/first-service/**
        - id: second-service
          uri: http://localhost:8082/
          predicates:
            - Path=/second-service/**
```
- 코드 설정
```java
@Configuration
public class FilterConfig {

    @Bean
    public RouteLocator gatewayRoutes(RouteLocatorBuilder builder) {
        return builder.routes()
                .route(r -> r.path("/first-service/**")
                        .uri("http://localhost:8081"))
                .route(r -> r.path("/second-service/**")
                        .uri("http://localhost:8082"))
                .build();
    }
}
```

### Filter 적용
- 게이트웨이에서 헤더를 추가하는 방식의 필터
- Java 코드로 추가
```java
    @Bean
public RouteLocator gatewayRoutes(RouteLocatorBuilder builder) {
    return builder.routes()
            .route(r -> r.path("/first-service/**")
                    .filters(f-> f.addRequestHeader("first-request","first-request-header")
                            .addResponseHeader("first-response","first-response-header"))
                    .uri("http://localhost:8081"))
            .route(r -> r.path("/second-service/**")
                    .filters(f-> f.addRequestHeader("second-request","second-request-header")
                            .addResponseHeader("second-response","second-response-header"))
                    .uri("http://localhost:8082"))
            .build();
}
``` 
    - PreFilter 에서 addRequestHeader 가 실행됨
    - PostFilter 에서 addResponseHeader 가 실행됨

- yml 에서 추가
```yaml
spring:
  application:
    name: gateway-service
  cloud:
    gateway:
      routes:
        - id: first-service
          uri: http://localhost:8081/
          predicates:
            - Path=/first-service/**
          filters:
            - AddRequestHeader=first-request, first-request-header2
            - AddResponseHeader=first-response, first-response-header2
        - id: second-service
          uri: http://localhost:8082/
          predicates:
            - Path=/second-service/**
          filters:
            - AddRequestHeader=second-request, second-request-header2
            - AddResponseHeader=second-response, second-response-header2
```
- filters 속성을 활용하여 추가

### Custom Filter
- 커스텀 필터 추가하기
- AbstractGatewayFilterFactory 클래스를 상속받아 apply 메소드를 오버라이드하여 사용
```java
@Slf4j
@Component
public class CustomFilter extends AbstractGatewayFilterFactory<CustomFilter.Config> {

    public CustomFilter() {
        super(Config.class);
    }

    @Override
    public GatewayFilter apply(Config config) {
        //PreFilter
        return ((exchange, chain) -> {
            //http.server.reactive
            //reactive 의존성 필요
            ServerHttpRequest request = exchange.getRequest();
            ServerHttpResponse response = exchange.getResponse();

            log.info("Custom Pre Filter : request id -> {}", request);

            //Post Filter
            return chain.filter(exchange).then(Mono.fromRunnable(() -> {
                log.info("Custom Post Filter : response code -> {}", response.getStatusCode());
            }));
        });
    }

    public static class Config {
        // Put the Configuration Properties
    }
}
```
- 추가 설정 정보가 필요하다면 Config 클래스에 필드를 추가 가능
- 특정 경로에만 추가를 원하는 경우 아래처럼 사용
```yaml
  cloud:
    gateway:
      routes:
        - id: first-service
          uri: http://localhost:8081/
          predicates:
            - Path=/first-service/**
          filters:
            - CustomFilter
        - id: second-service
          uri: http://localhost:8082/
          predicates:
            - Path=/second-service/**
          filters:
            - CustomFilter
```
- 특정 경로에만 원하는 필터 사용 가능
- 전역적인 사용을 원하는 경우 아래처럼 구성
```yaml
  application:
    name: gateway-service
  cloud:
    gateway:
      default-filters:
        - name: GlobalFilter
          args:
            baseMessage: Spring Gate Way Global Filter
            preLogger: true
            postLogger: true
```
- 필터의 순서를 조절하고 싶은 경우 OrderedGatewayFilter 를 구현체로 사용하여 순서를 넣어줄 수 있음
```java
    @Override
    public GatewayFilter apply(Config config) {
        GatewayFilter filter = new OrderedGatewayFilter((exchange, chain) -> {
            ServerHttpRequest request = exchange.getRequest();
            ServerHttpResponse response = exchange.getResponse();

            log.info("Logging Filter Base Message : {}", config.baseMessage);

            if (config.isPreLogger()){
                log.info("Logging PRE Filter Start : request id -> {}", request.getId());
            }

            //Post Filter
            return chain.filter(exchange).then(Mono.fromRunnable(() -> {
                if (config.isPostLogger()){
                    log.info("Logging POST Filter End : request id -> {}", response.getStatusCode());
                }
            }));
        }, OrderedGatewayFilter.LOWEST_PRECEDENCE);
//        }, OrderedGatewayFilter.HIGHEST_PRECEDENCE);//필터의 우선순위를 정하는 매개변수
        return filter;
    }
```

### API Gateway 와 Eureka 의 연동
- gateway 서버의 yml에 유레카 정보 등록
```yaml
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8761/eureka # 유레카 서버 정보
```
- 대상 서버의 uri를 입력
```yaml
spring:
  application:
    name: gateway-service
  cloud:
    gateway:
      routes:
        - id: first-service
          uri: lb://FIRST-SERVICE #lb://유레카에 등록된 서비스 이름
          predicates:
            - Path=/first-service/**
```
- lb : 로드밸런싱을 의미함