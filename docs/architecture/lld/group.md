# Group Module LLD

## 1. R&R
- 그룹 생성, 관리, 멤버 초대/삭제
- 그룹 내 권한 관리

## 2. API/프로토콜
### REST API
- POST /api/group
- GET /api/group/{id}
- PATCH /api/group/{id}
- DELETE /api/group/{id}
- POST /api/group/{id}/invite

## 3. 내부 로직
- 그룹 생성 시 유저/권한 연동
- 멤버 초대/삭제 시 알림 처리
- 그룹 삭제 시 연관 데이터 정리


## 4. Dependency
- User 모듈
- Notification Service

## 5. Data Scheme

### Group Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 그룹 ID        |
| name       | STRING   | 그룹명         |
| owner_id   | UUID     | 생성자(유저) ID|
| created_at | DATETIME | 생성일         |
| updated_at | DATETIME | 수정일         |

### GroupMember Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 멤버 ID        |
| group_id   | UUID     | 그룹 ID        |
| user_id    | UUID     | 유저 ID        |
| role       | STRING   | 역할(학생/튜터)|
| joined_at  | DATETIME | 가입일         |
