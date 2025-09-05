# Monitoring Module LLD

## 1. R&R
- 수업/학습 현황 모니터링
- 통계/리포트 제공

## 2. API/프로토콜
### REST API
- GET /api/monitoring/overview
- GET /api/monitoring/classroom/{id}
- GET /api/monitoring/group/{id}

## 3. 내부 로직
- 실시간 데이터 집계
- 통계 데이터 가공/리포트 생성


## 4. Dependency
- Classroom 모듈
- Group 모듈
- DB(통계 데이터)

## 5. Data Scheme

### MonitoringLog Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 로그 ID        |
| target_id  | UUID     | 대상 ID(수업/그룹 등)|
| type       | STRING   | 유형(출석/활동 등)|
| data       | JSON     | 상세 데이터    |
| timestamp  | DATETIME | 기록 시각      |

### MonitoringStat Table
| 필드명     | 타입     | 설명           |
| ---------- | -------- | -------------- |
| id         | UUID     | 통계 ID        |
| target_id  | UUID     | 대상 ID        |
| type       | STRING   | 통계 유형      |
| value      | FLOAT    | 통계 값        |
| period     | STRING   | 집계 기간      |
