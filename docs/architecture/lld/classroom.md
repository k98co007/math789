# Classroom Module LLD

## 1. R&R
- 수업 생성, 라이브 진행, 이력 관리
- 수업 내 문제/QA/Shorts/Variant 등 관리

## 2. API/프로토콜
### REST API
- POST /api/classroom
- GET /api/classroom/{id}
- GET /api/classroom/history
- POST /api/classroom/live
- POST /api/classroom/problem
- POST /api/classroom/qa

## 3. 내부 로직
- 수업 생성 시 커리큘럼/그룹 연동
- 라이브 세션 관리(시작/종료/참여자)
- 문제/QA 등록 및 이력 저장


## 4. Dependency
- Curriculum 모듈
- Group 모듈
- User 모듈
- 실시간 서버(WebSocket 등)

## 5. Data Scheme

### Classroom Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 수업 ID        |
| curriculum_id| UUID   | 커리큘럼 ID    |
| group_id   | UUID     | 그룹 ID        |
| teacher_id | UUID     | 담당자(유저) ID|
| status     | STRING   | 상태(예정/진행/종료)|
| start_time | DATETIME | 시작 시간      |
| end_time   | DATETIME | 종료 시간      |
| created_at | DATETIME | 생성일         |
| updated_at | DATETIME | 수정일         |

### ClassroomHistory Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 이력 ID        |
| classroom_id| UUID    | 수업 ID        |
| event      | STRING   | 이벤트(입장/퇴장 등)|
| user_id    | UUID     | 유저 ID        |
| timestamp  | DATETIME | 발생 시각      |

### ClassroomProblem Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 문제 ID        |
| classroom_id| UUID    | 수업 ID        |
| content    | JSON     | 문제 내용      |
| order      | INT      | 순서           |
