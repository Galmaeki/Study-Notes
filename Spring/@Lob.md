## @Lob
- Large Object 를 의미
- JPA 와 함께 사용됨
- 큰 크기의 데이터를 담아야 하는 컬럼에 붙여 사용하는 어노테이션
- Lob 형식은 주로 2가지로 구분됨
  - BLOB(Binary Large Object) : 이진 데이터를 저장하는데 사용
  - CLOB (Character Large Object) : 문자 데이터를 저장하는 데 사용
- 주로 게시글의 Content 부분이나 상품 설명같은 많은 내용을 담는 부분에 사용됨
- DB에 따라 LOB 를 지원하는 방식이 다름
  - MySQL: `BLOB`, `TEXT`, `MEDIUMTEXT`, `LONGTEXT`
  - PostgreSQL: `BYTEA`, `TEXT`
  - Oracle: `BLOB`, `CLOB`

### 주의
- 대용량 데이터일 가능성이 높으므로 지연 로딩을 사용하면 성능 저하를 예방 할 수 있음
```java
    @Lob
    @Basic(fetch = FetchType.LAZY)
    @Column(name = "content")
    private String content;
```
