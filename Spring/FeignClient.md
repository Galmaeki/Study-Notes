## FeignClient
- Spring Cloud 에서 제공하는 Http Client
- Http 호출을 쉽게 할 수 있도록 도와줌
- RestClient 나 WebClient 를 사용해서 RestApi를 호출하는것 보다 더 간결한 코드를 작성 가능

### 특징
- 인터페이스 기반 API 호출
- Spring Cloud와 연동하여 Ribbon(로드밸런싱) Eureka(서비스 디스커버리) 등 과 쉽게 통합 가능
- 자동 Json 직렬화, 역직렬화 지원
- 실패시 재시도 및 오류처리에 대해 쉽게 구성 가능
- 비동기로 작동함

#### 장점
- 간결하고 직관적인 API 정의
- 다양한 기능 지원
- 쉬운 통합
- 확장성
- Spring Cloud 의존성과 완벽한 연동

#### 단점
- RestTemplate 대비 세부적인 설정이 복잡함
- 직접적인 Http 통신 제어가 어려움

### 사용법
1. 의존성 추가
```groovy
dependencies {
    implementation 'org.springframework.cloud:spring-cloud-starter-openfeign'
}
```
2. Feign Client 활성화
```java
@EnableFeignClients
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```
3. Feign Client 인터페이스 생성
   - name : 실제 구현체가 Application Context 에 빈으로 등록 될 때 이름으로 사용됨
   - url : 요청을 보낼 엔드포인트
   - MethodMapping : 해당 Http 메소드로 요청을 보냄
   - RequestParam & PathVariable : 요청과 함께 보낼 값
   - RequestHeader : 지정한 헤더가 포함되어 요청됨
   - 어노테이션을 사용하지 않은 매개변수는 Body에 포함됨
```java
@FeignClient(name = "user-service", url = "http://localhost:8080")
public interface UserClient {

    @GetMapping("/users/{id}")
    UserResponse getUserById(@PathVariable("id") Long id, 
                             @RequestParam("param") String param, 
                             @RequestParam(value = "code") String code,
                             @RequestHeader("Authorization") String token);
}
```
4. 사용
```java
@Service
@RequireArgumentConstructor
public class UserService {

    private final UserClient userClient;

    public UserResponse getUserInfo(Long id) {
        return userClient.getUserById(id);
    }
}
```

### 예외 처리
1. try-catch 사용
```java
    String get() {
        String result = null;
        try {
            result = client.get();
        } catch (FeignException e) {
            //예외 처리
        }
        return result;
    }
```
2. Custom ErrorDecoder 사용
  -  ErrorDecoder 를 상속받아 Custom ErrorDecoder 클래스를 작성
```java
public class FeignErrorDecoder implements ErrorDecoder {
   @Override
   public Exception decode(String s, Response response) {
      switch (response.status()) {
         case 400:
            return new ResponseStatusException(HttpStatusCode.valueOf(response.status())
                    , "400 에러 발생");
         case 404:
            return new ResponseStatusException(HttpStatusCode.valueOf(response.status())
                 , "404 에러 발생");
      }
      return null;
   }
}
```
  - Bean 등록
```java
    @Bean
    public FeignErrorDecoder feignErrorDecoderFactory(){
        return new FeignErrorDecoder();
    }
```
