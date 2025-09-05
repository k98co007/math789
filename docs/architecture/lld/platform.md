# Platform Module LLD

## 1. R&R
- 공통 기능(로그, 알림, 파일, 결제 등)
- 시스템 설정/관리

## 2. API/프로토콜
### REST API
- POST /api/platform/log
- POST /api/platform/notify
- POST /api/platform/upload
- POST /api/platform/payment

## 3. 내부 로직
- 로그 기록, 알림 발송, 파일 업로드, 결제 처리
- 공통 유틸/미들웨어 제공


## 4. Dependency
- 외부 서비스(결제, 파일, 알림 등)
- User 모듈(권한)

## 5. Data Scheme

### Log Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 로그 ID        |
| user_id    | UUID     | 유저 ID        |
| action     | STRING   | 액션/이벤트    |
| data       | JSON     | 상세 데이터    |
| timestamp  | DATETIME | 기록 시각      |

### Notification Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 알림 ID        |
| user_id    | UUID     | 수신자(유저) ID|
| type       | STRING   | 알림 유형      |
| message    | STRING   | 메시지         |
| read       | BOOLEAN  | 읽음 여부      |
| created_at | DATETIME | 생성일         |

### File Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 파일 ID        |
| user_id    | UUID     | 업로더(유저) ID|
| url        | STRING   | 파일 URL       |
| type       | STRING   | 파일 유형      |
| created_at | DATETIME | 업로드일       |

### Payment Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 결제 ID        |
| user_id    | UUID     | 결제자(유저) ID|
| amount     | INT      | 결제 금액      |
| status     | STRING   | 결제 상태      |
| method     | STRING   | 결제 수단      |
| created_at | DATETIME | 결제일         |
