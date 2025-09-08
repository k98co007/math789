# 모듈별 상세 API 명세 및 데이터 모델

---

## 1. User Module


### API 명세 (상세)

#### POST /user/register : 회원가입
- 설명: 신규 사용자 회원가입
- 권한: 비로그인
- 요청 예시:
```json
{
  "email": "test@example.com",
  "password": "string",
  "name": "홍길동"
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "email": "test@example.com",
  "name": "홍길동",
  "role": "student"
}
```
- 에러 코드:
  - 400: 필수값 누락/형식 오류
  - 409: 이메일 중복
  - 500: 서버 오류

#### POST /user/login : 로그인
- 설명: 이메일/비밀번호로 로그인
- 권한: 비로그인
- 요청 예시:
```json
{
  "email": "test@example.com",
  "password": "string"
}
```
- 응답 예시:
```json
{
  "token": "jwt-token",
  "user": {
    "id": "uuid",
    "email": "test@example.com",
    "name": "홍길동",
    "role": "student"
  }
}
```
- 에러 코드:
  - 401: 인증 실패(이메일/비밀번호 불일치)
  - 500: 서버 오류

#### GET /user/info/{userId} : 사용자 정보 조회
- 설명: 특정 사용자 정보 조회
- 권한: 로그인(본인 또는 관리자)
- 응답 예시:
```json
{
  "id": "uuid",
  "email": "test@example.com",
  "name": "홍길동",
  "role": "student",
  "createdAt": "2025-09-08T12:00:00Z"
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 사용자 없음
  - 500: 서버 오류

#### PUT /user/info/{userId} : 사용자 정보 수정
- 설명: 사용자 정보(이름 등) 수정
- 권한: 로그인(본인)
- 요청 예시:
```json
{
  "name": "홍길동2"
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "name": "홍길동2"
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 사용자 없음
  - 500: 서버 오류

#### GET /user/role/{userId} : 권한 조회
- 설명: 사용자 권한 조회
- 권한: 로그인(본인 또는 관리자)
- 응답 예시:
```json
{
  "role": "student"
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 사용자 없음

#### PUT /user/role/{userId} : 권한 변경
- 설명: 사용자 권한 변경(관리자만 가능)
- 권한: 관리자
- 요청 예시:
```json
{
  "role": "tutor"
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "role": "tutor"
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 사용자 없음
  - 400: 잘못된 role 값



### 데이터 모델 (상세)

#### User
| 필드명      | 타입      | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------- | ---- | ------------------ | ------------------- | ------------------- |
| id          | string    | Y    | PK, UUID           | 사용자 고유 ID      | "b1c2..."           |
| email       | string    | Y    | Unique, Email      | 이메일              | "test@example.com"  |
| password    | string    | Y    | 해시저장           | 비밀번호(해시)      | "$2b$10$..."        |
| name        | string    | Y    |                    | 이름                | "홍길동"            |
| role        | enum      | Y    | student/tutor/admin| 권한                | "student"           |
| createdAt   | datetime  | Y    |                    | 생성일시            | "2025-09-08T12:00:00Z" |
| updatedAt   | datetime  | Y    |                    | 수정일시            | "2025-09-08T12:00:00Z" |

#### User 예시
```json
{
  "id": "b1c2...",
  "email": "test@example.com",
  "password": "$2b$10$...",
  "name": "홍길동",
  "role": "student",
  "createdAt": "2025-09-08T12:00:00Z",
  "updatedAt": "2025-09-08T12:00:00Z"
}
```

---

## 2. Curriculum Module


### API 명세 (상세)

#### POST /curriculum/create : 커리큘럼 생성
- 설명: 신규 커리큘럼 생성
- 권한: 로그인(튜터/관리자)
- 요청 예시:
```json
{
  "title": "수학 기초",
  "description": "기초 수학 과정",
  "contents": [
    { "type": "lecture", "data": { "title": "1강" } }
  ]
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "title": "수학 기초",
  "description": "기초 수학 과정",
  "createdBy": "userId",
  "createdAt": "2025-09-08T12:00:00Z"
}
```
- 에러 코드:
  - 400: 필수값 누락/형식 오류
  - 403: 권한 없음
  - 500: 서버 오류

#### GET /curriculum/list : 커리큘럼 목록 조회
- 설명: 전체 커리큘럼 목록 조회(필터/검색 지원 가능)
- 권한: 로그인
- 응답 예시:
```json
[
  { "id": "uuid", "title": "수학 기초", "description": "기초 수학 과정" }
]
```
- 에러 코드:
  - 401: 인증 필요
  - 500: 서버 오류

#### GET /curriculum/detail/{curriculumId} : 커리큘럼 상세 조회
- 설명: 특정 커리큘럼 상세 정보 조회
- 권한: 로그인
- 응답 예시:
```json
{
  "id": "uuid",
  "title": "수학 기초",
  "description": "기초 수학 과정",
  "contents": [
    { "id": "cid1", "type": "lecture", "data": { "title": "1강" } }
  ]
}
```
- 에러 코드:
  - 404: 커리큘럼 없음
  - 500: 서버 오류

#### PUT /curriculum/update/{curriculumId} : 커리큘럼 수정
- 설명: 커리큘럼 정보 수정
- 권한: 로그인(생성자/관리자)
- 요청 예시:
```json
{
  "title": "수학 기초(수정)",
  "description": "업데이트된 설명"
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "title": "수학 기초(수정)",
  "description": "업데이트된 설명"
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 커리큘럼 없음
  - 500: 서버 오류

#### DELETE /curriculum/delete/{curriculumId} : 커리큘럼 삭제
- 설명: 커리큘럼 삭제
- 권한: 로그인(생성자/관리자)
- 응답 예시:
```json
{
  "success": true
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 커리큘럼 없음
  - 500: 서버 오류



### 데이터 모델 (상세)

#### Curriculum
| 필드명      | 타입           | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------------| ---- | ------------------ | ------------------- | ------------------- |
| id          | string        | Y    | PK, UUID           | 커리큘럼 고유 ID    | "c1d2..."           |
| title       | string        | Y    |                    | 제목                | "수학 기초"         |
| description | string        | N    |                    | 설명                | "기초 수학 과정"    |
| contents    | array[Content]| N    |                    | 콘텐츠 목록         | [{...}]              |
| createdBy   | string        | Y    | FK(User)            | 생성자(유저ID)      | "b1c2..."           |
| createdAt   | datetime      | Y    |                    | 생성일시            | "2025-09-08T12:00:00Z" |
| updatedAt   | datetime      | Y    |                    | 수정일시            | "2025-09-08T12:00:00Z" |

#### Content
| 필드명      | 타입      | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------- | ---- | ------------------ | ------------------- | ------------------- |
| id          | string    | Y    | PK, UUID           | 콘텐츠 고유 ID      | "ct1..."            |
| type        | enum      | Y    | lecture/quiz/assignment| 콘텐츠 유형     | "lecture"           |
| data        | object    | Y    |                    | 콘텐츠 데이터       | {"title": "1강"}   |

#### Curriculum 예시
```json
{
  "id": "c1d2...",
  "title": "수학 기초",
  "description": "기초 수학 과정",
  "contents": [
    { "id": "ct1...", "type": "lecture", "data": { "title": "1강" } }
  ],
  "createdBy": "b1c2...",
  "createdAt": "2025-09-08T12:00:00Z",
  "updatedAt": "2025-09-08T12:00:00Z"
}
```

---

## 3. Group Module


### API 명세 (상세)

#### POST /group/create : 그룹 생성
- 설명: 신규 그룹 생성
- 권한: 로그인(튜터/관리자)
- 요청 예시:
```json
{
  "name": "수학반",
  "description": "수학 심화반"
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "name": "수학반",
  "description": "수학 심화반",
  "createdAt": "2025-09-08T12:00:00Z"
}
```
- 에러 코드:
  - 400: 필수값 누락/형식 오류
  - 403: 권한 없음
  - 500: 서버 오류

#### GET /group/list : 그룹 목록 조회
- 설명: 전체 그룹 목록 조회(필터/검색 지원 가능)
- 권한: 로그인
- 응답 예시:
```json
[
  { "id": "uuid", "name": "수학반", "description": "수학 심화반" }
]
```
- 에러 코드:
  - 401: 인증 필요
  - 500: 서버 오류

#### GET /group/detail/{groupId} : 그룹 상세 조회
- 설명: 특정 그룹 상세 정보 조회
- 권한: 로그인(멤버)
- 응답 예시:
```json
{
  "id": "uuid",
  "name": "수학반",
  "description": "수학 심화반",
  "members": ["userId1", "userId2"]
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 그룹 없음
  - 500: 서버 오류

#### PUT /group/update/{groupId} : 그룹 정보 수정
- 설명: 그룹 정보(이름/설명) 수정
- 권한: 로그인(생성자/관리자)
- 요청 예시:
```json
{
  "name": "수학반(수정)",
  "description": "업데이트된 설명"
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "name": "수학반(수정)",
  "description": "업데이트된 설명"
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 그룹 없음
  - 500: 서버 오류

#### DELETE /group/delete/{groupId} : 그룹 삭제
- 설명: 그룹 삭제
- 권한: 로그인(생성자/관리자)
- 응답 예시:
```json
{
  "success": true
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 그룹 없음
  - 500: 서버 오류

#### POST /group/invite : 멤버 초대
- 설명: 그룹에 멤버 초대
- 권한: 로그인(생성자/관리자)
- 요청 예시:
```json
{
  "groupId": "uuid",
  "userId": "userId"
}
```
- 응답 예시:
```json
{
  "success": true
}
```
- 에러 코드:
  - 400: 필수값 누락/형식 오류
  - 403: 권한 없음
  - 404: 그룹/유저 없음
  - 409: 이미 멤버
  - 500: 서버 오류

#### GET /group/member/{groupId} : 멤버 목록 조회
- 설명: 그룹 멤버 목록 조회
- 권한: 로그인(멤버)
- 응답 예시:
```json
[
  { "userId": "userId1", "role": "student" },
  { "userId": "userId2", "role": "tutor" }
]
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 그룹 없음
  - 500: 서버 오류



### 데이터 모델 (상세)

#### Group
| 필드명      | 타입           | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------------| ---- | ------------------ | ------------------- | ------------------- |
| id          | string        | Y    | PK, UUID           | 그룹 고유 ID        | "g1d2..."           |
| name        | string        | Y    | Unique             | 그룹명              | "수학반"            |
| description | string        | N    |                    | 설명                | "수학 심화반"       |
| members     | array[string] | N    | FK(User)           | 멤버 유저ID 목록    | ["b1c2...", ...]    |
| createdAt   | datetime      | Y    |                    | 생성일시            | "2025-09-08T12:00:00Z" |
| updatedAt   | datetime      | Y    |                    | 수정일시            | "2025-09-08T12:00:00Z" |

#### Group 예시
```json
{
  "id": "g1d2...",
  "name": "수학반",
  "description": "수학 심화반",
  "members": ["b1c2...", "b1c3..."],
  "createdAt": "2025-09-08T12:00:00Z",
  "updatedAt": "2025-09-08T12:00:00Z"
}
```

---

## 4. Monitoring Module


### API 명세 (상세)

#### GET /monitoring/status : 서비스 상태 조회
- 설명: 전체 서비스 상태(서버, DB 등) 조회
- 권한: 관리자
- 응답 예시:
```json
{
  "status": "ok",
  "db": "ok",
  "message": "All systems operational"
}
```
- 에러 코드:
  - 403: 권한 없음
  - 500: 서버 오류

#### GET /monitoring/log : 활동 로그 조회
- 설명: 사용자/시스템 활동 로그 목록 조회(필터/검색 지원)
- 권한: 관리자
- 응답 예시:
```json
[
  { "id": "uuid", "userId": "userId", "action": "login", "timestamp": "2025-09-08T12:00:00Z" }
]
```
- 에러 코드:
  - 403: 권한 없음
  - 500: 서버 오류

#### POST /monitoring/alert : 알림/경고 등록
- 설명: 시스템/사용자 알림 또는 경고 등록
- 권한: 관리자
- 요청 예시:
```json
{
  "type": "warning",
  "message": "서버 부하 발생"
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "type": "warning",
  "message": "서버 부하 발생",
  "createdAt": "2025-09-08T12:00:00Z"
}
```
- 에러 코드:
  - 400: 필수값 누락/형식 오류
  - 403: 권한 없음
  - 500: 서버 오류



### 데이터 모델 (상세)

#### Log
| 필드명      | 타입      | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------- | ---- | ------------------ | ------------------- | ------------------- |
| id          | string    | Y    | PK, UUID           | 로그 고유 ID        | "l1d2..."           |
| userId      | string    | N    | FK(User)           | 관련 유저 ID        | "b1c2..."           |
| action      | string    | Y    |                    | 액션/이벤트         | "login"             |
| timestamp   | datetime  | Y    |                    | 발생 시각           | "2025-09-08T12:00:00Z" |
| details     | object    | N    |                    | 상세 데이터         | {"ip": "1.2.3.4"}  |

#### Alert
| 필드명      | 타입      | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------- | ---- | ------------------ | ------------------- | ------------------- |
| id          | string    | Y    | PK, UUID           | 알림/경고 고유 ID   | "a1d2..."           |
| type        | enum      | Y    | info/warning/error | 알림/경고 유형      | "warning"           |
| message     | string    | Y    |                    | 메시지              | "서버 부하 발생"    |
| createdAt   | datetime  | Y    |                    | 생성일시            | "2025-09-08T12:00:00Z" |

#### Log 예시
```json
{
  "id": "l1d2...",
  "userId": "b1c2...",
  "action": "login",
  "timestamp": "2025-09-08T12:00:00Z",
  "details": {"ip": "1.2.3.4"}
}
```

#### Alert 예시
```json
{
  "id": "a1d2...",
  "type": "warning",
  "message": "서버 부하 발생",
  "createdAt": "2025-09-08T12:00:00Z"
}
```

---

## 5. Tutoring Module


### API 명세 (상세)

#### POST /tutoring/session : 세션 예약
- 설명: 튜터링 세션 예약(학생이 요청)
- 권한: 로그인(학생)
- 요청 예시:
```json
{
  "tutorId": "uuid",
  "startTime": "2025-09-09T10:00:00Z",
  "endTime": "2025-09-09T11:00:00Z"
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "tutorId": "uuid",
  "studentId": "uuid",
  "startTime": "2025-09-09T10:00:00Z",
  "endTime": "2025-09-09T11:00:00Z",
  "status": "scheduled"
}
```
- 에러 코드:
  - 400: 필수값 누락/형식 오류
  - 403: 권한 없음
  - 409: 시간 중복/튜터 불가
  - 500: 서버 오류

#### GET /tutoring/session/{sessionId} : 세션 조회
- 설명: 튜터링 세션 상세 조회
- 권한: 로그인(참여자)
- 응답 예시:
```json
{
  "id": "uuid",
  "tutorId": "uuid",
  "studentId": "uuid",
  "startTime": "2025-09-09T10:00:00Z",
  "endTime": "2025-09-09T11:00:00Z",
  "status": "scheduled"
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 세션 없음
  - 500: 서버 오류

#### PUT /tutoring/session/{sessionId} : 세션 수정
- 설명: 세션 일정/상태 수정
- 권한: 로그인(참여자)
- 요청 예시:
```json
{
  "startTime": "2025-09-09T11:00:00Z",
  "endTime": "2025-09-09T12:00:00Z"
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "startTime": "2025-09-09T11:00:00Z",
  "endTime": "2025-09-09T12:00:00Z"
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 세션 없음
  - 409: 시간 중복
  - 500: 서버 오류

#### DELETE /tutoring/session/{sessionId} : 세션 취소
- 설명: 세션 취소(학생/튜터)
- 권한: 로그인(참여자)
- 응답 예시:
```json
{
  "success": true
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 세션 없음
  - 500: 서버 오류

#### POST /tutoring/match : 튜터-학생 매칭
- 설명: 튜터-학생 자동 매칭 요청
- 권한: 로그인(학생)
- 요청 예시:
```json
{
  "criteria": { "subject": "수학", "time": "2025-09-09T10:00:00Z" }
}
```
- 응답 예시:
```json
{
  "tutorId": "uuid",
  "matched": true
}
```
- 에러 코드:
  - 404: 매칭 실패
  - 500: 서버 오류

#### POST /tutoring/feedback : 피드백 등록
- 설명: 세션 종료 후 피드백 등록
- 권한: 로그인(참여자)
- 요청 예시:
```json
{
  "sessionId": "uuid",
  "content": "좋은 수업 감사합니다."
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "sessionId": "uuid",
  "authorId": "uuid",
  "content": "좋은 수업 감사합니다.",
  "createdAt": "2025-09-09T12:00:00Z"
}
```
- 에러 코드:
  - 400: 필수값 누락/형식 오류
  - 403: 권한 없음
  - 404: 세션 없음
  - 500: 서버 오류



### 데이터 모델 (상세)

#### Session
| 필드명      | 타입      | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------- | ---- | ------------------ | ------------------- | ------------------- |
| id          | string    | Y    | PK, UUID           | 세션 고유 ID        | "s1d2..."           |
| tutorId     | string    | Y    | FK(User)           | 튜터(유저) ID       | "b1c2..."           |
| studentId   | string    | Y    | FK(User)           | 학생(유저) ID       | "b1c3..."           |
| startTime   | datetime  | Y    |                    | 시작 시간           | "2025-09-09T10:00:00Z" |
| endTime     | datetime  | Y    |                    | 종료 시간           | "2025-09-09T11:00:00Z" |
| status      | enum      | Y    | scheduled/completed/cancelled | 상태 | "scheduled" |

#### Feedback
| 필드명      | 타입      | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------- | ---- | ------------------ | ------------------- | ------------------- |
| id          | string    | Y    | PK, UUID           | 피드백 고유 ID      | "f1d2..."           |
| sessionId   | string    | Y    | FK(Session)        | 세션 ID             | "s1d2..."           |
| authorId    | string    | Y    | FK(User)           | 작성자(유저) ID     | "b1c2..."           |
| content     | string    | Y    |                    | 피드백 내용         | "좋은 수업 감사합니다." |
| createdAt   | datetime  | Y    |                    | 생성일시            | "2025-09-09T12:00:00Z" |

#### Session 예시
```json
{
  "id": "s1d2...",
  "tutorId": "b1c2...",
  "studentId": "b1c3...",
  "startTime": "2025-09-09T10:00:00Z",
  "endTime": "2025-09-09T11:00:00Z",
  "status": "scheduled"
}
```

#### Feedback 예시
```json
{
  "id": "f1d2...",
  "sessionId": "s1d2...",
  "authorId": "b1c2...",
  "content": "좋은 수업 감사합니다.",
  "createdAt": "2025-09-09T12:00:00Z"
}
```

---

## 6. Platform Module


### API 명세 (상세)

#### POST /platform/notify : 알림 발송
- 설명: 사용자에게 알림 발송(이메일/SMS/푸시)
- 권한: 관리자/시스템
- 요청 예시:
```json
{
  "userId": "uuid",
  "type": "email",
  "message": "새로운 공지가 있습니다."
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "userId": "uuid",
  "type": "email",
  "message": "새로운 공지가 있습니다.",
  "createdAt": "2025-09-08T12:00:00Z"
}
```
- 에러 코드:
  - 400: 필수값 누락/형식 오류
  - 403: 권한 없음
  - 404: 유저 없음
  - 500: 서버 오류

#### POST /platform/payment : 결제 처리
- 설명: 결제 요청 및 처리
- 권한: 로그인(사용자)
- 요청 예시:
```json
{
  "userId": "uuid",
  "amount": 10000,
  "method": "card"
}
```
- 응답 예시:
```json
{
  "id": "uuid",
  "userId": "uuid",
  "amount": 10000,
  "status": "pending",
  "createdAt": "2025-09-08T12:00:00Z"
}
```
- 에러 코드:
  - 400: 필수값 누락/형식 오류
  - 403: 권한 없음
  - 402: 결제 실패
  - 500: 서버 오류

#### GET /platform/statistics : 통계 조회
- 설명: 서비스 통계 데이터 조회
- 권한: 관리자
- 응답 예시:
```json
{
  "id": "uuid",
  "type": "userCount",
  "data": { "count": 100 },
  "createdAt": "2025-09-08T12:00:00Z"
}
```
- 에러 코드:
  - 403: 권한 없음
  - 500: 서버 오류

#### GET /platform/report : 리포트 조회
- 설명: 서비스 리포트(월간/주간 등) 조회
- 권한: 관리자
- 응답 예시:
```json
{
  "report": "<html>...</html>"
}
```
- 에러 코드:
  - 403: 권한 없음
  - 500: 서버 오류



### 데이터 모델 (상세)

#### Notification
| 필드명      | 타입      | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------- | ---- | ------------------ | ------------------- | ------------------- |
| id          | string    | Y    | PK, UUID           | 알림 고유 ID        | "n1d2..."           |
| userId      | string    | Y    | FK(User)           | 수신자(유저) ID     | "b1c2..."           |
| message     | string    | Y    |                    | 메시지              | "새로운 공지"       |
| type        | enum      | Y    | email/sms/push     | 알림 유형           | "email"             |
| createdAt   | datetime  | Y    |                    | 생성일시            | "2025-09-08T12:00:00Z" |

#### Payment
| 필드명      | 타입      | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------- | ---- | ------------------ | ------------------- | ------------------- |
| id          | string    | Y    | PK, UUID           | 결제 고유 ID        | "p1d2..."           |
| userId      | string    | Y    | FK(User)           | 결제자(유저) ID     | "b1c2..."           |
| amount      | number    | Y    | >=0                | 결제 금액           | 10000                |
| status      | enum      | Y    | pending/completed/failed | 결제 상태   | "pending"            |
| createdAt   | datetime  | Y    |                    | 결제일시            | "2025-09-08T12:00:00Z" |

#### Statistics
| 필드명      | 타입      | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------- | ---- | ------------------ | ------------------- | ------------------- |
| id          | string    | Y    | PK, UUID           | 통계 고유 ID        | "s1d2..."           |
| type        | string    | Y    |                    | 통계 유형           | "userCount"         |
| data        | object    | Y    |                    | 통계 데이터         | {"count": 100}      |
| createdAt   | datetime  | Y    |                    | 생성일시            | "2025-09-08T12:00:00Z" |

#### Notification 예시
```json
{
  "id": "n1d2...",
  "userId": "b1c2...",
  "message": "새로운 공지",
  "type": "email",
  "createdAt": "2025-09-08T12:00:00Z"
}
```

#### Payment 예시
```json
{
  "id": "p1d2...",
  "userId": "b1c2...",
  "amount": 10000,
  "status": "pending",
  "createdAt": "2025-09-08T12:00:00Z"
}
```

#### Statistics 예시
```json
{
  "id": "s1d2...",
  "type": "userCount",
  "data": { "count": 100 },
  "createdAt": "2025-09-08T12:00:00Z"
}
```

---

## 7. Classroom Module


### API 명세 (상세)

#### POST /classroom/enter : 교실 입장
- 설명: 교실(수업) 입장
- 권한: 로그인(학생/교사)
- 요청 예시:
```json
{
  "classroomId": "uuid"
}
```
- 응답 예시:
```json
{
  "success": true
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 교실 없음
  - 409: 이미 입장
  - 500: 서버 오류

#### GET /classroom/lesson/{classroomId} : 수업 콘텐츠 조회
- 설명: 교실의 수업 콘텐츠(강의 등) 조회
- 권한: 로그인(멤버)
- 응답 예시:
```json
{
  "lessons": [ { "id": "lid1", "title": "1강" } ]
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 교실 없음
  - 500: 서버 오류

#### GET /classroom/problem/{classroomId} : 문제 목록/상세 조회
- 설명: 교실의 문제 목록 및 상세 조회
- 권한: 로그인(멤버)
- 응답 예시:
```json
{
  "problems": [ { "id": "pid1", "content": "2+2=?" } ]
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 교실 없음
  - 500: 서버 오류

#### POST /classroom/problem/submit : 문제 풀이 제출
- 설명: 문제 풀이 제출
- 권한: 로그인(학생)
- 요청 예시:
```json
{
  "problemId": "pid1",
  "answer": "4"
}
```
- 응답 예시:
```json
{
  "correct": true,
  "score": 10
}
```
- 에러 코드:
  - 400: 필수값 누락/형식 오류
  - 403: 권한 없음
  - 404: 문제 없음
  - 500: 서버 오류

#### POST /classroom/python/execute : 파이썬 코드 실행 (Playground)
- 설명: 파이썬 코드 실행 결과 반환
- 권한: 로그인(멤버)
- 요청 예시:
```json
{
  "classroomId": "uuid",
  "code": "print(1+1)"
}
```
- 응답 예시:
```json
{
  "result": "2",
  "error": null
}
```
- 에러 코드:
  - 400: 코드 누락
  - 403: 권한 없음
  - 500: 실행 오류

#### POST /classroom/python/step : 파이썬 코드 단계별 실행
- 설명: 파이썬 코드 단계별 실행 결과 반환
- 권한: 로그인(멤버)
- 요청 예시:
```json
{
  "classroomId": "uuid",
  "code": "for i in range(3): print(i)"
}
```
- 응답 예시:
```json
{
  "stepTrace": [ { "line": 1, "output": "0" }, { "line": 1, "output": "1" } ]
}
```
- 에러 코드:
  - 400: 코드 누락
  - 403: 권한 없음
  - 500: 실행 오류

#### GET /classroom/python/trace/{sessionId} : 변수 추적/실행 과정 조회
- 설명: 파이썬 실행 세션의 변수/실행 과정 추적
- 권한: 로그인(멤버)
- 응답 예시:
```json
{
  "variables": { "i": 2 },
  "stepTrace": [ { "line": 1, "output": "0" } ]
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 세션 없음
  - 500: 서버 오류

#### POST /classroom/qna : 질문 등록
- 설명: 교실 내 Q&A 질문 등록
- 권한: 로그인(멤버)
- 요청 예시:
```json
{
  "classroomId": "uuid",
  "question": "이 문제 풀이가 맞나요?"
}
```
- 응답 예시:
```json
{
  "id": "qid1",
  "question": "이 문제 풀이가 맞나요?",
  "createdAt": "2025-09-08T12:00:00Z"
}
```
- 에러 코드:
  - 400: 필수값 누락/형식 오류
  - 403: 권한 없음
  - 404: 교실 없음
  - 500: 서버 오류

#### GET /classroom/qna/{classroomId} : 질문 목록/상세 조회
- 설명: 교실 내 Q&A 목록 및 상세 조회
- 권한: 로그인(멤버)
- 응답 예시:
```json
[
  { "id": "qid1", "question": "이 문제 풀이가 맞나요?" }
]
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 교실 없음
  - 500: 서버 오류

#### POST /classroom/live/request : 실시간 강의 요청
- 설명: 실시간 강의 요청(학생)
- 권한: 로그인(학생)
- 요청 예시:
```json
{
  "classroomId": "uuid"
}
```
- 응답 예시:
```json
{
  "success": true
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 교실 없음
  - 409: 이미 요청됨
  - 500: 서버 오류

#### GET /classroom/shorts/{problemId} : 쇼츠 강의 조회
- 설명: 문제별 쇼츠(짧은 강의) 조회
- 권한: 로그인(멤버)
- 응답 예시:
```json
{
  "shorts": [ { "id": "sid1", "title": "문제 해설" } ]
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 문제 없음
  - 500: 서버 오류

#### GET /classroom/variant/{problemId} : 변형 문제 조회
- 설명: 문제별 변형 문제 조회
- 권한: 로그인(멤버)
- 응답 예시:
```json
{
  "variants": [ { "id": "vid1", "content": "2+3=?" } ]
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 문제 없음
  - 500: 서버 오류

#### GET /classroom/prerequisite/{problemId} : 선수 학습 항목 조회
- 설명: 문제별 선수 학습 항목 조회
- 권한: 로그인(멤버)
- 응답 예시:
```json
{
  "prerequisites": [ { "id": "pid2", "title": "덧셈 기초" } ]
}
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 문제 없음
  - 500: 서버 오류

#### GET /classroom/activity/{userId} : 활동 이력 조회
- 설명: 사용자의 교실 내 활동 이력 조회
- 권한: 로그인(본인/교사/관리자)
- 응답 예시:
```json
[
  { "id": "aid1", "type": "problem", "detail": { "score": 10 } }
]
```
- 에러 코드:
  - 403: 권한 없음
  - 404: 사용자 없음
  - 500: 서버 오류



### 데이터 모델 (상세)

#### Classroom
| 필드명      | 타입           | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------------| ---- | ------------------ | ------------------- | ------------------- |
| id          | string        | Y    | PK, UUID           | 교실 고유 ID        | "cl1d2..."          |
| name        | string        | Y    |                    | 교실명              | "1학년 1반"         |
| students    | array[string] | N    | FK(User)           | 학생 유저ID 목록    | ["b1c2...", ...]    |
| teachers    | array[string] | N    | FK(User)           | 교사 유저ID 목록    | ["b1c3...", ...]    |
| lessons     | array[string] | N    | FK(Lesson)         | 수업(강의) ID 목록  | ["l1d2...", ...]    |
| problems    | array[string] | N    | FK(Problem)        | 문제 ID 목록        | ["p1d2...", ...]    |
| createdAt   | datetime      | Y    |                    | 생성일시            | "2025-09-08T12:00:00Z" |
| updatedAt   | datetime      | Y    |                    | 수정일시            | "2025-09-08T12:00:00Z" |

#### PythonPlaygroundSession
| 필드명      | 타입      | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------- | ---- | ------------------ | ------------------- | ------------------- |
| sessionId   | string    | Y    | PK, UUID           | 세션 고유 ID        | "ps1d2..."           |
| userId      | string    | Y    | FK(User)           | 실행자(유저) ID     | "b1c2..."           |
| classroomId | string    | Y    | FK(Classroom)      | 교실 ID             | "cl1d2..."          |
| code        | string    | Y    |                    | 실행 코드           | "print(1+1)"         |
| result      | string    | N    |                    | 실행 결과           | "2"                  |
| stepTrace   | array[object]| N  |                    | 단계별 실행 정보    | [{"line":1,"output":"2"}]|
| variables   | object    | N    |                    | 변수 정보           | {"i":2}              |
| error       | string/null| N    |                    | 에러 메시지         | null                 |
| createdAt   | datetime  | Y    |                    | 생성일시            | "2025-09-08T12:00:00Z" |

#### ClassroomActivity
| 필드명      | 타입      | 필수 | 제약조건           | 설명                | 예시                |
| ----------- | --------- | ---- | ------------------ | ------------------- | ------------------- |
| id          | string    | Y    | PK, UUID           | 활동 고유 ID        | "a1d2..."           |
| userId      | string    | Y    | FK(User)           | 유저 ID             | "b1c2..."           |
| classroomId | string    | Y    | FK(Classroom)      | 교실 ID             | "cl1d2..."          |
| type        | enum      | Y    | lesson/problem/python/qna/live/shorts/variant/prerequisite | 활동 유형 | "problem" |
| detail      | object    | N    |                    | 상세 정보           | {"score":10}        |
| createdAt   | datetime  | Y    |                    | 생성일시            | "2025-09-08T12:00:00Z" |

#### Classroom 예시
```json
{
  "id": "cl1d2...",
  "name": "1학년 1반",
  "students": ["b1c2..."],
  "teachers": ["b1c3..."],
  "lessons": ["l1d2..."],
  "problems": ["p1d2..."],
  "createdAt": "2025-09-08T12:00:00Z",
  "updatedAt": "2025-09-08T12:00:00Z"
}
```
