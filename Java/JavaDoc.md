## Java Doc
- Java에서 /** 내용 */로 작성하는 주석
- Java 소스코드에서 API문서를 Html 태그 형식으로 작성하게 해주는 도구
- 컴파일 시 모든 주석은 지워지므로 성능엔 영향이 없음

#### 사용법
- @return
  - 반환 값에 대한 설명
- @param
  - 매개변수에 대한 설명
```java
/**
     * 클라이언트에게 데이터를 전달 할 때 사용<p>
     * @param data 클라이언트에 전달 할 데이터<br>
     * 메시지 : Success
     */
    public static <T> ResponseEntity<ApiResponse<T>> ok(T data) {
        return ResponseEntity.ok(
                ApiResponse.<T>builder()
                        .status(HttpStatus.OK.value())
                        .data(data)
                        .message("Success")
                        .build()
        );
    }
```
- @link
  - 참조에 대한 링크 제공
  - {} 내부에 사용하고 뒤에 링크할 내용 입력
```java
    /**
     * 예외 발생시 클라이언트에게 알릴 때 사용<p>
     * @param errorCode 에러코드 목록은 {@link ErrorCode}을 참조<p>
     *                  원하는 에러코드가 없을 때, ErrorCode 새로 추가
     *                  또는{@link ApiResponse#errorCustom(HttpStatus, String) errorCustom} 중 선택하여 사용
     */
    public static <T> ResponseEntity<ApiResponse<T>> error(ErrorCode errorCode) {
        return ResponseEntity.status(errorCode.getHttpStatus())
                .body(
                        ApiResponse.<T>builder()
                                .status(errorCode.getHttpStatus().value())
                                .data(null)
                                .message(errorCode.getMessage())
                                .build()
                );
    }
```