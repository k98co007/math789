# 아키텍처 레이어 설계

- Presentation Layer: Web/Mobile 클라이언트, 관리자 대시보드
- API Gateway Layer: 인증, 라우팅, Rate Limiting
- Service Layer: 각 도메인별 비즈니스 로직 (User, Curriculum, Group, Monitoring, Tutoring, Platform, Classroom)
    - Classroom: 온라인 수업, 문제 풀이, 질문/답변, 실시간 강의, 쇼츠 강의, 변형 문제, 선수 학습, 활동 이력, 통합형 파이썬 코딩 환경(Playground) 비즈니스 로직 포함
    - Python Playground: 실시간 코드 실행, 시각적 피드백, 단계별 실행, 변수 추적, 오류 안내 등 인터랙티브 코딩 학습 지원
- Data Layer: 도메인별 DB (UserDB, CurriculumDB 등), 외부 연동 API
- Integration Layer: 외부 인증, 결제, 통계, 알림 시스템 연동

각 레이어는 독립적으로 배포 가능하며, API Gateway를 통해 통합 관리됩니다.
