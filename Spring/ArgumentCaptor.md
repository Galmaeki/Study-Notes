## ArgumentCaptor
- 메소드에 인자를 받기 전 중간값을 검증하고 싶을 때 사용하는 클래스
- 테스트 하려는 메소드 외부의 인수에 접근하기 힘들 때 유용함

### 사용법
```java
        void successTest() {
            //Given
            given(memberRepository.findByLoginId(anyString()))
                    .willReturn(Optional.of(authorMember));

            given(postRepository.findById(anyLong()))
                    .willReturn(Optional.of(post));

            //중간 객체의 검증을 위한 메소드
            ArgumentCaptor<Post> capturedPost = ArgumentCaptor.forClass(Post.class);

            //When
            postService.postUpdate(editedRequest, post.getId(), authorMember.getLoginId());

            //Then
            verify(postRepository).save(capturedPost.capture());

            Post replacedPost = capturedPost.getValue();
            assertThat(replacedPost.getTitle()).isEqualTo(editedRequest.title());
            assertThat(replacedPost.getContent()).isEqualTo(editedRequest.content());
        }
```
ArgumentCaptor