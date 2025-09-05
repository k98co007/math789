# 모듈별 상세 API 명세 및 데이터 모델

---

## 1. User Module

### API 명세
- POST /user/register : 회원가입
- POST /user/login : 로그인
- GET /user/info/{userId} : 사용자 정보 조회
- PUT /user/info/{userId} : 사용자 정보 수정
- GET /user/role/{userId} : 권한 조회
- PUT /user/role/{userId} : 권한 변경

### 데이터 모델
```json
{
  "User": {
    "id": "string",
    "email": "string",
    "password": "string",
    "name": "string",
    "role": "enum[student, tutor, admin]",
    "createdAt": "datetime",
    "updatedAt": "datetime"
  }
}
```

---

## 2. Curriculum Module

### API 명세
- POST /curriculum/create : 커리큘럼 생성
- GET /curriculum/list : 커리큘럼 목록 조회
- GET /curriculum/detail/{curriculumId} : 커리큘럼 상세 조회
- PUT /curriculum/update/{curriculumId} : 커리큘럼 수정
- DELETE /curriculum/delete/{curriculumId} : 커리큘럼 삭제

### 데이터 모델
```json
{
  "Curriculum": {
    "id": "string",
    "title": "string",
    "description": "string",
    "contents": "array[Content]",
    "createdBy": "userId",
    "createdAt": "datetime",
    "updatedAt": "datetime"
  },
  "Content": {
    "id": "string",
    "type": "enum[lecture, quiz, assignment]",
    "data": "object"
  }
}
```

---

## 3. Group Module

### API 명세
- POST /group/create : 그룹 생성
- GET /group/list : 그룹 목록 조회
- GET /group/detail/{groupId} : 그룹 상세 조회
- PUT /group/update/{groupId} : 그룹 정보 수정
- DELETE /group/delete/{groupId} : 그룹 삭제
- POST /group/invite : 멤버 초대
- GET /group/member/{groupId} : 멤버 목록 조회

### 데이터 모델
```json
{
  "Group": {
    "id": "string",
    "name": "string",
    "description": "string",
    "members": "array[userId]",
    "createdAt": "datetime",
    "updatedAt": "datetime"
  }
}
```

---

## 4. Monitoring Module

### API 명세
- GET /monitoring/status : 서비스 상태 조회
- GET /monitoring/log : 활동 로그 조회
- POST /monitoring/alert : 알림/경고 등록

### 데이터 모델
```json
{
  "Log": {
    "id": "string",
    "userId": "string",
    "action": "string",
    "timestamp": "datetime",
    "details": "object"
  },
  "Alert": {
    "id": "string",
    "type": "enum[info, warning, error]",
    "message": "string",
    "createdAt": "datetime"
  }
}
```

---

## 5. Tutoring Module

### API 명세
- POST /tutoring/session : 세션 예약
- GET /tutoring/session/{sessionId} : 세션 조회
- PUT /tutoring/session/{sessionId} : 세션 수정
- DELETE /tutoring/session/{sessionId} : 세션 취소
- POST /tutoring/match : 튜터-학생 매칭
- POST /tutoring/feedback : 피드백 등록

### 데이터 모델
```json
{
  "Session": {
    "id": "string",
    "tutorId": "string",
    "studentId": "string",
    "startTime": "datetime",
    "endTime": "datetime",
    "status": "enum[scheduled, completed, cancelled]"
  },
  "Feedback": {
    "id": "string",
    "sessionId": "string",
    "authorId": "string",
    "content": "string",
    "createdAt": "datetime"
  }
}
```

---

## 6. Platform Module

### API 명세
- POST /platform/notify : 알림 발송
- POST /platform/payment : 결제 처리
- GET /platform/statistics : 통계 조회
- GET /platform/report : 리포트 조회

### 데이터 모델
```json
{
  "Notification": {
    "id": "string",
    "userId": "string",
    "message": "string",
    "type": "enum[email, sms, push]",
    "createdAt": "datetime"
  },
  "Payment": {
    "id": "string",
    "userId": "string",
    "amount": "number",
    "status": "enum[pending, completed, failed]",
    "createdAt": "datetime"
  },
  "Statistics": {
    "id": "string",
    "type": "string",
    "data": "object",
    "createdAt": "datetime"
  }
}
```

---

## 7. Classroom Module

### API 명세
- POST /classroom/enter : 교실 입장
- GET /classroom/lesson/{classroomId} : 수업 콘텐츠 조회
- GET /classroom/problem/{classroomId} : 문제 목록/상세 조회
- POST /classroom/problem/submit : 문제 풀이 제출
- POST /classroom/python/execute : 파이썬 코드 실행 (Playground)
- POST /classroom/python/step : 파이썬 코드 단계별 실행
- GET /classroom/python/trace/{sessionId} : 변수 추적/실행 과정 조회
- POST /classroom/qna : 질문 등록
- GET /classroom/qna/{classroomId} : 질문 목록/상세 조회
- POST /classroom/live/request : 실시간 강의 요청
- GET /classroom/shorts/{problemId} : 쇼츠 강의 조회
- GET /classroom/variant/{problemId} : 변형 문제 조회
- GET /classroom/prerequisite/{problemId} : 선수 학습 항목 조회
- GET /classroom/activity/{userId} : 활동 이력 조회

### 데이터 모델
```json
{
  "Classroom": {
    "id": "string",
    "name": "string",
    "students": ["userId"],
    "teachers": ["userId"],
    "lessons": ["lessonId"],
    "problems": ["problemId"],
    "createdAt": "datetime",
    "updatedAt": "datetime"
  },
  "PythonPlaygroundSession": {
    "sessionId": "string",
    "userId": "string",
    "classroomId": "string",
    "code": "string",
    "result": "string",
    "stepTrace": ["object"],
    "variables": "object",
    "error": "string|null",
    "createdAt": "datetime"
  },
  "ClassroomActivity": {
    "id": "string",
    "userId": "string",
    "classroomId": "string",
    "type": "enum[lesson, problem, python, qna, live, shorts, variant, prerequisite]",
    "detail": "object",
    "createdAt": "datetime"
  }
}
```
