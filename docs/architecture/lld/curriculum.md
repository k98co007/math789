# Curriculum Module LLD

## 1. R&R
- 커리큘럼/콘텐츠 목록 관리
- 커리큘럼 상세, 생성, 수정, 삭제

## 2. API/프로토콜
### REST API
- GET /api/curriculum/list
- GET /api/curriculum/{id}
- POST /api/curriculum
- PATCH /api/curriculum/{id}
- DELETE /api/curriculum/{id}

## 3. 내부 로직
- 커리큘럼 생성/수정 시 입력값 검증
- 커리큘럼 삭제 시 연관 데이터 체크
- 목록 조회 시 페이징/필터링 처리


## 4. Dependency
- DB(Curriculum, Content Table)
- User 모듈(권한 체크)

## 5. Data Scheme

### Curriculum Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 커리큘럼 ID    |
| title      | STRING   | 제목           |
| description| STRING   | 설명           |
| owner_id   | UUID     | 생성자(유저) ID|
| created_at | DATETIME | 생성일         |
| updated_at | DATETIME | 수정일         |

### Content Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 콘텐츠 ID      |
| curriculum_id| UUID   | 커리큘럼 ID    |
| type       | STRING   | 유형(문제/자료 등)|
| data       | JSON     | 콘텐츠 데이터  |
| order      | INT      | 순서           |
