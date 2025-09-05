# 모듈 및 R&R

## 1. User Module
- R&R: 회원가입, 로그인, 정보조회/수정, 권한 관리
- API: /user/register, /user/login, /user/info, /user/role
- Protocol: REST, OAuth2 (외부 인증 연동)

## 2. Curriculum Module
- R&R: 커리큘럼 CRUD, 목록/상세 조회, 콘텐츠 관리
- API: /curriculum/create, /curriculum/list, /curriculum/detail, /curriculum/content
- Protocol: REST

## 3. Group Module
- R&R: 그룹 생성/수정/삭제, 멤버 초대/관리, 활동 내역 관리
- API: /group/create, /group/invite, /group/member, /group/activity
- Protocol: REST

## 4. Monitoring Module
- R&R: 실시간 상태 모니터링, 로그 수집/분석, 알림/경고
- API: /monitoring/status, /monitoring/log, /monitoring/alert
- Protocol: REST, WebSocket (실시간)

## 5. Tutoring Module
- R&R: 세션 예약/관리, 튜터-학생 매칭, 피드백 관리
- API: /tutoring/session, /tutoring/match, /tutoring/feedback
- Protocol: REST, WebSocket (실시간)

## 6. Platform Module
- R&R: 알림, 결제, 통계, 리포트 제공
- API: /platform/notify, /platform/payment, /platform/statistics, /platform/report
- Protocol: REST, 외부 API 연동

## 7. Classroom Module (분할)

### 7-1. Lesson Module
- R&R: 온라인 수업 입장, 수업 진행, 수업 콘텐츠 관리
- API: /classroom/enter, /classroom/lesson
- Protocol: REST, WebSocket (실시간)

### 7-2. Problem Solving Module
- R&R: 문제 풀이, 변형 문제 제공, 풀이 기록 관리
- API: /classroom/problem, /classroom/variant
- Protocol: REST

### 7-3. QnA Module
- R&R: 질문/답변, 토론, 피드백
- API: /classroom/qna
- Protocol: REST, WebSocket (실시간)

### 7-4. Live Lecture Module
- R&R: 실시간 강의, 실시간 상호작용, 출석 관리
- API: /classroom/live
- Protocol: WebSocket (실시간)

### 7-5. Shorts Module
- R&R: 쇼츠 강의(짧은 동영상), 요약 콘텐츠 제공
- API: /classroom/shorts
- Protocol: REST

### 7-6. Prerequisite Module
- R&R: 선수 학습 콘텐츠 제공, 선수 학습 이력 관리
- API: /classroom/prerequisite
- Protocol: REST

### 7-7. Activity Tracking Module
- R&R: 활동 이력 관리, 학습 진도/통계 제공
- API: /classroom/activity
- Protocol: REST

### 7-8. Python Playground Module
- R&R: 실시간 파이썬 코드 실행, 시각적 피드백, 단계별 실행, 변수 추적, 오류 안내 등 인터랙티브 코딩 학습 지원
- API: /classroom/python, /classroom/python/execute, /classroom/python/step, /classroom/python/trace
- Protocol: WebSocket (실시간), 내부 실행 엔진

