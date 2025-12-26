## 페이지네이션
- 네트워크 자원을 효율적으로 이용하기 위해 데이터를 정렬기준에 따라 분할하여 가져오는것

### 오프셋 기반 페이지네이션(OffSet-Based-Pagination)
-  ~에서 ~까지 요청
- 요청 예시
```http request
GET /posts?page=3&size=20&sort=createdAt,desc
```
- 쿼리
```sql
SELECT *
FROM posts
ORDER BY created_at DESC
LIMIT 20 OFFSET 60;
```
#### 장점
- 구현 난이도가 낮음
- 특정 페이지로 이동하는 ui와 잘 어울림
- 전체 페이지의 수를 알 수 있음

#### 단점
- 데이터가 변경될 때의 중복/누락
  - 쓰기 빈도가 빈번한 테이블인 경우, 이미 본 데이터가 중복되어 보일 수 있음
  - 중간에 데이터가 삭제된 경우 누락되어 못 본 데이터가 생길 수 있음
- Offset 쿼리의 퍼포먼스 이슈
  - Offset 쿼리는 위치를 계산한 후 필요 데이터를 찾을때까지 데이터를 스캔 한 후 지정된 갯수만큼 내보내는 방식
  - 예를들어 Offset 60000 일 때 DB는 앞의 6만개를 읽고 버린 뒤의 나머지 데이터를 내보내 비용이 증가함 

### 커서 기반 페이지네이션(Cursor-Based-Pagination)
- 마지막지점 이후 지점을 요청
- 요청 예시
```http request
GET /posts?size=20&cursor=2025-12-26T09:00:00Z_12345
```
- 쿼리
```sql
SELECT *
FROM posts
WHERE (created_at, id) < (:lastCreatedAt, :lastId)
ORDER BY created_at DESC, id DESC
LIMIT 20;
```
#### 장점
- 데이터가 많아도 성능이 일정
- 중복과 누락이 오프셋 기반과 비교하여 적음
#### 단점
- 특정 페이지로 이동 UI를 사용할 수 없음
- 구현 난이도가 비교적 높음
  - cursor 값으로 사용되는 데이터 집합은 테이블 상에서 유일성이 보장되어야함
    - ex)created_at로만 정렬하는 경우 동일한 순간에 생성된 쿼리가 존재 할 때, 데이터 일관성을 보장 불가
  - created_at 과 id 로 정렬을 하는 경우 인덱스의 크기 증가와 구현의 복잡도가 늘어나게됨

### JPA에서
- JPA에서 자동으로 만들어지는 findPageBy나 findSliceBy는 모두 오프셋 기반
- 리턴타입을 Page or Slice로 적용하고 Pageable 을 받도록 작성
```java
public interface PostRepository extends JpaRepository<Post, Long> {
    Page<Post> findAllById(Long id, Pageable pageable);
}
```