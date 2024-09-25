## @Nested
- 어노테이션이 붙은 클래스를 둘러싸고 있는 클래스의 인스턴스와 설정 및 상태를 공유 할 수 있는 중첩된 비정적(non-static) 테스트 클래스
- 정적 클래스가 아닌 이너 클래스 에만 사용 가능함
- 내부 클래스는 '@BeforeAll' 과 '@AfterAll' 을 제외하고 라이프 사이클이 지원됨
- 계층적으로 테스트를 작성하여 테스트 그룹화가 쉬워지고, 테스트를 이해하기 쉬워짐

### 예제
- 실제로 사용했던 예시 코드
```java
    @Nested
    @DisplayName("getDeferredResult()는")
    class Context_getDeferredResult{

        @DisplayName("DeferredResult 반환에 성공한다.")
        @Test
        void _willSuccess() {
            // given
            Long memberId = 1L;

            // when&then
            assertThat(alarmService.getDeferredResult(memberId)).isNotNull();
        }

        @DisplayName("타임 아웃이 발생한다.")
        @Test
        void _willTimeOut() {
            // given
            Long memberId = 1L;

            //when
            DeferredResult<ResponseEntity<ResponseDTO<AlarmResponseDto>>> result = alarmService.getDeferredResult(memberId);
            result.setErrorResult(ResponseEntity.status(HttpStatus.NO_CONTENT).body(ResponseDTO.ok()));

            // then
            result.onTimeout(() -> {
                assertTrue(result.hasResult());
                assertEquals(HttpStatus.NO_CONTENT, ((ResponseEntity<ResponseDTO<AlarmResponseDto>>) Objects.requireNonNull(result.getResult())).getStatusCode());
            });
        }
    }
```