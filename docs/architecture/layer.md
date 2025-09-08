# 아키텍처 레이어 설계

- Presentation Layer: Web/Mobile 클라이언트, 관리자 대시보드
- API Gateway Layer: 인증, 라우팅, Rate Limiting
- Service Layer: 각 도메인별 비즈니스 로직 (User, Curriculum, Group, Monitoring, Tutoring, Platform, Classroom)
    - Classroom: 온라인 수업, 문제 풀이, 질문/답변, 실시간 강의, 쇼츠 강의, 변형 문제, 선수 학습, 활동 이력, 통합형 파이썬 코딩 환경(Playground) 비즈니스 로직 포함
    - Python Playground: 실시간 코드 실행, 시각적 피드백, 단계별 실행, 변수 추적, 오류 안내 등 인터랙티브 코딩 학습 지원
- Data Layer: 도메인별 DB (UserDB, CurriculumDB 등), 외부 연동 API
- Integration Layer: 외부 인증, 결제, 통계, 알림 시스템 연동


## 각 레이어별 운영/보안/테스트/확장성 가이드 (추가)

- **Presentation Layer**: 사용자 입력 검증, XSS/CSRF 방지, 접근성/반응형 UI, 에러/로딩/빈 상태 안내
- **API Gateway Layer**: 인증/인가(JWT, OAuth2), Rate Limiting, 요청/응답 로깅, 장애 시 Graceful Degradation
- **Service Layer**: 비즈니스 로직 단위 테스트, 예외 상황(권한, 데이터 없음 등) 명확 처리, 트랜잭션 관리
- **Data Layer**: 데이터 무결성, 백업/복구, 마이그레이션, 성능 튜닝, 민감정보 암호화
- **Integration Layer**: 외부 서비스 장애 격리, 재시도/타임아웃, 연동 로그, 보안키 관리

> 각 레이어별로 위 항목을 반드시 고려하여 설계/구현/운영할 것
