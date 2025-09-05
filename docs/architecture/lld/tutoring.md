# Tutoring Module LLD

## 1. R&R
- 튜터링 세션 생성, 관리
- 튜터-학생 매칭, 일정 관리

## 2. API/프로토콜
### REST API
- POST /api/tutoring/session
- GET /api/tutoring/session/{id}
- PATCH /api/tutoring/session/{id}
- DELETE /api/tutoring/session/{id}

## 3. 내부 로직
- 세션 생성 시 튜터/학생 매칭
- 일정 충돌 체크
- 세션 상태 변경(예약, 진행, 종료)


## 4. Dependency
- User 모듈
- Calendar/Notification Service

## 5. Data Scheme

### TutoringSession Table
| 필드명         | 타입        | 설명                |
| -------------- | ----------- | ------------------- |
| id             | UUID        | 세션 고유 ID        |
| tutor_id       | UUID        | 튜터(유저) ID       |
| student_id     | UUID        | 학생(유저) ID       |
| start_time     | DATETIME    | 시작 시간           |
| end_time       | DATETIME    | 종료 시간           |
| status         | STRING      | 상태(예약/진행/종료)|
| created_at     | DATETIME    | 생성일              |
| updated_at     | DATETIME    | 수정일              |

### TutoringSessionLog Table (옵션)
| 필드명         | 타입        | 설명                |
| -------------- | ----------- | ------------------- |
| id             | UUID        | 로그 고유 ID        |
| session_id     | UUID        | 튜터링 세션 ID      |
| action         | STRING      | 상태 변경/이벤트    |
| timestamp      | DATETIME    | 이벤트 발생 시각    |
