- 자바에서 코틀린으로 작성한 객체의 Getter와 Setter에 대한 접근 방법
---
### Java 에서 Kotlin 에 대한 접근
#### Getter, Setter
- 가변변수에는 바로 getter, setter를 통해 접근 가능
  - 가변변수에 setter를 숨기려면 private set 키워드 사용
    - 이런 경우, 변수 자체는 가변 변수이므로 내부에서 조작은 가능
- 불변변수에는 getter 만 가능
```kotlin
class Student {
    var name: String? = null
    var birthDate: LocalDate? = null
    
    val age: Int = 5
}
```
```java
    public static void main(String[] args) {
    Student student = new Student();
    student.setName("재현");
    student.setBirthDate(LocalDate.now());
//        student.setGrade(5);//컴파일 에러
//        student.setAge(10);//컴파일 에러

    System.out.println(student.getBirthDate());
    System.out.println(student.getName());
    System.out.println(student.getGrade());
    System.out.println(student.getAge());
}

```

#### Property 접근
- @JvmField 어노테이션을 사용하여 바로 필드 접근도 가능
```kotlin
class Student {

    @JvmField
    var name: String? = null
    var birthDate: LocalDate? = null

    var grade: Int = 5
        private set

    val age: Int = 5
}
```
```java
    public static void main(String[] args) throws IOException {
        Student student = new Student();
//        student.setName("재현");//컴파일 에러
        student.name = "재현";
        student.setBirthDate(LocalDate.now());
//        student.setGrade(5);//컴파일 에러
//        student.setAge(10);//컴파일 에러

        System.out.println(student.getBirthDate());
//        System.out.println(student.getName());//컴파일 에러
        System.out.println(student.name);
        System.out.println(student.getGrade());
        System.out.println(student.getAge());
    }
```
- **@JvmField :** Kotlin 컴파일러에게 이 속성에 대한 getter/setter를 생성하지 않고 필드로 노출하도록 지시합니다.

## Kotlin 에서 Java 에 대한 접근
#### Getter, Setter
- Kotlin 에서는 Java 의 getter, setter 를 바로 호출 가능
  - 하지만 권장되지는 않음
- 프로퍼티를 통해 바로 접근 할 수도 있음
- Propertiy 가 존재하지 않아도 Getter, Setter 메소드라면 프로퍼티처럼 접근이 가능
- Setter 가 존재해도 이름이 set프로퍼티 형식이 아니라면 불가능
- Getter 가 존재해도 이름이 get프로퍼티 형식이 아니라면 불가능
```java
@Getter
@Setter
public class Student {
    private String name;
    private int age;
    private String address;
    public String getUuid(){
      return "UUID";
    }
    public void setUuid(String string){
      System.out.println(string);
    }
}
```
```kotlin
fun main() {
    //Java Style
    val 학생1 = Student()
    학생1.setName("재현")
    학생1.setAge(66)
    학생1.setAddress("서울")

    println(학생1.getName())
    println(학생1.getAge())
    println(학생1.getAddress())

    //Kotlin Style
    val 학생2 = Student();
    학생2.name = "재영"
    학생2.age = 55
    학생2.address = "부산"

    println(학생2.name)
    println(학생2.age)
    println(학생2.address)

    //Property 가 없어도 Getter 라면 프로퍼티처럼 사용 가능
    println(학생1.uuid)
    println(학생1.setUuid("세터없음"))
}
```

