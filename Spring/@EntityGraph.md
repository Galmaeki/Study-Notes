## EntityGraph
- 연관관계가 있는 엔티티를 어떻게 로딩할 지 지정하는 어노테이션
- 특정 상황에서 필요한 연관 엔티티만 즉시 로딩 가능
- LazyLoading 문제와 N+1 문제 해결

### 사용
- @EntityGraph 어노테이션을 Jpa 메소드에 사용
- 즉시 로딩 할 엔티티만 attributePaths 에 명시
```java
@Entity
public class Post {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "member_id")
    private Member member;

    @OneToMany(mappedBy = "post", fetch = FetchType.LAZY)
    private List<HashtagMap> hashtagMaps;
}
```
```java
public interface PostRepository extends JpaRepository<Post, Long> {
    
    @EntityGraph(attributePaths = {"member", "hashtagMaps"})
    Optional<Post> findById(Long id);
}
```
- attributePaths 에 필드 명을 지정하여 사용
  - 위 코드의 경우 Post 에서 필드로 지정한 member 와 hashtagMaps 를 지정함
```java
@Getter
@Entity
@NoArgsConstructor
public class HashtagMap {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Setter
    @ManyToOne
    @JoinColumn(name = "postId")
    private Post post;

    @ManyToOne
    @JoinColumn(name = "hashtagId")
    private Hashtag hashtag;

    public HashtagMap(Hashtag hashtag) {
        this.hashtag = hashtag;
    }
}
```
- 위 처럼 연관관계를 더 거치는 경우 아래처럼 . 을 사용해서 하위 경로를 명시하여 사용
```java
public interface PostRepository extends JpaRepository<Post, Long> {

    @EntityGraph(attributePaths = {"hashtagMaps", "hashtagMaps.hashtag"})
    Page<Post> findAll(Pageable pageable);
}
```

### 장점
- 지연 로딩과 즉시 로딩의 유연한 제어를 통해 필요한 경우에만 즉시 로딩을 적용 가능
- Fetch를 통한 쿼리 수 감소

### 단점
- 쿼리가 복잡해 질 수 있음
  - 복잡한 쿼리로 성능저하가 일어날 수 있음