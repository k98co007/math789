# UI 전체 레이아웃 및 페이지별 세부 레이아웃 설계

## 1. 전체 레이아웃 구조

- **Header(상단 네비게이션 바)**: 서비스 로고, 주요 메뉴, 사용자 정보, 알림 등 표시
- **Sidebar(좌측 사이드바)**: 주요 메뉴 및 페이지 이동 네비게이션
- **Main Content(메인 컨텐츠 영역)**: 각 페이지별 주요 내용 표시
- **Footer(하단, 선택)**: 저작권, 추가 정보 등

```
+------------------------------------------------------+
|                      Header                          |
+-------------------+----------------------------------+
|     Sidebar       |         Main Content             |
|  (Navigation)     |   (각 페이지별 세부 화면)        |
+-------------------+----------------------------------+
|                      Footer (선택)                  |
+------------------------------------------------------+
```

---

## 2. 페이지별 세부 레이아웃

### 2.1. Classroom 관련 페이지
- **공통:** 상단에 Classroom 탭/서브네비, 메인에 각 세부 컨텐츠
- **예시:**
  - classroom_live: 실시간 강의 정보, 채팅/질문 패널
  - classroom_history: 과거 강의 목록 및 상세
  - classroom_problem: 문제 리스트, 풀이 입력 영역
  - classroom_qa: Q&A 목록, 질문/답변 입력

### 2.2. Curriculum 관련 페이지
- curriculum_list: 커리큘럼 목록, 필터/검색, 상세 진입
- curriculum: 커리큘럼 상세, 강의/단원 리스트

### 2.3. Group 관련 페이지
- group_manage: 그룹 목록, 생성/수정/삭제, 멤버 관리
- group: 그룹 상세, 멤버 리스트, 활동 내역

### 2.4. Monitoring
- monitoring: 실시간/통계 대시보드, 차트, 필터

### 2.5. Tutoring
- tutoring: 튜터링 세션 목록, 예약, 진행중 세션

### 2.6. User 관련 페이지
- user_login, user_signup: 별도 Auth 레이아웃(중앙 정렬, 헤더/사이드바 없음)
- user_profile: 프로필 정보, 수정 폼

---

## 3. 레이아웃 적용 예시 (React/Next.js 기준)

- AppLayout (공통 레이아웃)
  - Header
  - Sidebar
  - MainContent (children)
  - Footer (선택)

- AuthLayout (로그인/회원가입 전용)
  - MainContent (중앙 정렬)

- ClassroomLayout (classroom 하위)
  - ClassroomTabs (서브네비)
  - MainContent (각 세부 페이지)

---

## 4. 참고
- 각 페이지별 세부 레이아웃은 UI/UX 요구사항에 따라 조정 가능
- 실제 구현 시, 프레임워크별 컴포넌트 구조에 맞게 분리 권장
