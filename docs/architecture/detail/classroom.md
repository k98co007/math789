# Classroom 도메인 상세 설계

## 1. 개요
- 온라인 수업, 문제풀이, Q&A, 실시간 강의 등 교실 관련 기능의 상세 설계 문서입니다.

## 2. 데이터 모델
- Classroom: id, name, schedule, status, created_at 등
- Problem: id, classroom_id, content, answer, type 등
- QA: id, classroom_id, user_id, question, answer, created_at 등
- LiveSession: id, classroom_id, start_time, end_time, status 등

## 3. 주요 기능 및 API
- 교실 입장/퇴장, 문제풀이, Q&A, 실시간 강의, 활동 이력 관리

## 4. 시나리오/플로우
- 교실 입장 → 문제풀이/Q&A/강의 참여 → 이력 저장

## 5. 예외/에러 처리
- 입장 실패, 문제 없음, 권한 없음 등

## 6. 기타
- 실시간 기능(WebSocket 등), 피드백/알림
