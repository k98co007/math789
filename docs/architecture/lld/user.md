# User Module LLD

## 1. R&R
- 사용자 회원가입, 로그인, 인증, 프로필 관리
- 권한 관리 및 세션 유지

## 2. API/프로토콜
### REST API
- POST /api/user/signup
- POST /api/user/login
- GET /api/user/profile
- PATCH /api/user/profile

### 예시
```json
POST /api/user/signup
{
  "email": "string",
  "password": "string",
  "name": "string"
}
```

## 3. 내부 로직
- 입력값 검증 → 비밀번호 해싱 → DB 저장
- 로그인 시 JWT 발급 및 세션 관리
- 프로필 조회/수정 시 인증 체크

## 4. Dependency

## 5. Data Scheme

### User Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 유저 고유 ID   |
| email      | STRING   | 이메일         |
| password   | STRING   | 해시된 비밀번호|
| name       | STRING   | 이름           |
| role       | STRING   | 권한(학생/튜터/관리자)|
| created_at | DATETIME | 생성일         |
| updated_at | DATETIME | 수정일         |

### UserProfile Table (옵션)
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 프로필 고유 ID |
| user_id    | UUID     | 유저 ID        |
| bio        | STRING   | 자기소개       |
| avatar_url | STRING   | 프로필 이미지  |

