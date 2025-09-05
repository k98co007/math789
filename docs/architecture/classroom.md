# 교실(Classroom) 아키텍처

이 문서는 수학교육 전문 서비스의 "교실(Classroom)" 기능에 대한 시스템 아키텍처를 설명합니다.

## 1. 개요
교실(Classroom)은 학생이 온라인 환경에서 수업, 문제 풀이, 질문, 실시간 강의 등 다양한 학습 활동을 수행할 수 있도록 지원하는 핵심 모듈입니다.

## 2. 주요 구성 요소
- **Classroom Service**: 교실 생성, 입장, 관리 등 교실의 기본 기능 제공
- **Lesson Module**: 수업 콘텐츠 제공 및 수강 이력 관리
- **Problem Solving Engine & Python Playground**: 수학 문제 풀이, 자동 채점, 파이썬 코딩 풀이 지원
    - 파이썬 코딩은 별도의 외부 툴이 아닌, 본 서비스와 완전히 통합된 인터랙티브 Playground 환경에서 제공됨
    - 학생은 Xcode의 Swift Playground와 유사하게, 코드를 입력하면 즉시 실행 과정과 결과를 시각적으로 확인할 수 있음
    - 단계별 실행, 변수 값 추적, 시각적 피드백, 오류 안내 등 실시간 학습 지원 기능 포함
- **Q&A Board**: 교실별 질문 게시판, 답변 및 소통 기능
- **Live Lecture Module**: 실시간(라이브) 강의 요청 및 진행, 1:1/그룹 지원
- **Shorts Lecture Module**: 문제별 쇼츠(Shorts) 강의 제공
- **Variant Problem Generator**: 변형 문제 생성 및 제공
- **Prerequisite Checker**: 문제별 선수 학습 항목 확인 및 연계 수업 제공
- **Activity Tracker**: 교실 내 학생 활동 이력 기록 및 통계 제공

## 3. 시스템 연동 구조
- **사용자(학생/선생님/튜터/학부모)** → 웹/앱 → Classroom Service
- Classroom Service는 Lesson, Problem Solving, Q&A, Live Lecture 등 각 모듈과 API 또는 내부 서비스로 연동
- Activity Tracker는 모든 학습 활동 데이터를 수집하여 통계 및 리포트 모듈과 연동

## 4. 데이터 흐름 예시
1. 학생이 교실에 입장 → Lesson Module에서 수업 콘텐츠 제공
2. 문제 풀이 선택 → Problem Solving Engine & Python Playground에서 문제 및 코딩 풀이, 자동 채점, 실시간 실행 및 시각적 피드백
3. 모르는 문제 질문 → Q&A Board에 게시, 답변 알림 수신
4. 실시간 강의 요청 → Live Lecture Module에서 선생님 연결 및 강의 진행
5. 문제별 쇼츠 강의 시청 → Shorts Lecture Module에서 영상 제공
6. 변형 문제 도전 → Variant Problem Generator에서 문제 제공
7. 선수 학습 필요 시 → Prerequisite Checker에서 관련 수업 연계
8. 모든 활동은 Activity Tracker에 기록

## 5. 확장 및 보안 고려사항
- 모듈별 마이크로서비스 구조로 확장성 확보
- 실시간 데이터 동기화 및 안정성
- 개인정보 및 학습 이력의 안전한 저장과 접근 제어

---
변경 및 추가 요구사항은 본 문서에 업데이트합니다.
