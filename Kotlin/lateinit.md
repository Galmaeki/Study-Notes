## lateinit
- 변수 선언과 동시에 값을 할당하기 어려울 때 lateinit 키워드를 사용해 변수를 나중에 초기화 할 수 있음
- 이 경우 null을 사용하지 않아도 선언 가능
- var 변수에만 사용 가능
  - val 사용 불가
- 초기화 전에 접근하면 UninitializedPropertyAccessException 예외 발생

### 예제 코드
```kotlin
@ExtendWith(SpringExtension::class)
class TodoServiceTests {

    @MockkBean
    lateinit var repository: TodoRepository

    //
    lateinit var todoService: TodoService

    @BeforeEach
    fun setUp(){
        todoService = TodoService(repository)
    }
}
```
- 위 코드의 경우 setUp 을 통해 service를 나중에 초기화 함