# Starworks - 근태 및 캘린더 모듈

> 대덕인재개발원 최종 프로젝트에서 **제가 담당한 코드**를 발췌한 포트폴리오입니다.  
> 본 저장소는 팀 프로젝트의 일부이며, **코드 샘플 전시**를 목적으로 실행을 전제로 하지 않습니다.

<br>

## 기술 스택

**Backend**  
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat&logo=java&logoColor=white)
![Spring](https://img.shields.io/badge/Spring-eGOV%204.3-6DB33F?style=flat&logo=spring)
![MyBatis](https://img.shields.io/badge/MyBatis-3.5-000000?style=flat)

**Frontend**  
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![D3.js](https://img.shields.io/badge/D3.js-F9A03C?style=flat&logo=d3.js)
![FullCalendar](https://img.shields.io/badge/FullCalendar-4285F4?style=flat)

**AI**  
![Google Cloud](https://img.shields.io/badge/Spring_AI-Vertex_AI-4285F4?style=flat&logo=googlecloud)

<br>

## 프로젝트 구조

```
src/main/java/kr/or/ddit/
├── attendance/
│   ├── controller/
│   │   ├── AttendanceController.java
│   │   ├── AttendanceStatsRestController.java
│   │   └── TimeAndAttendanceRestController.java
│   ├── service/
│   └── quartz/                    # 스케줄러 작업
│
├── calendar/
│   ├── controller/
│   │   └── CalendarController.java
│   ├── dept/                      # 부서 캘린더
│   ├── team/                      # 팀 캘린더
│   ├── users/                     # 개인 캘린더
│   └── front/
│
├── vacation/                      # 휴가 관리
├── businesstrip/                  # 출장 관리
├── vertex/                        # Vertex AI 연동
├── dto/                           # Data Transfer Object
└── vo/                            # Value Object

src/main/webapp/views/
├── attendance/
│   ├── attendance-main.jsp        # 개인 근태 화면
│   └── attendance-depart.jsp      # 부서 근태 화면
├── calendar/
│   ├── calendar-depart.jsp        # 부서 일정 화면
│   └── calendar-team.jsp          # 팀 일정 화면
└── ai-chat.jsp                    # AI 챗봇 화면

src/main/resources/static/js/
├── attendance/
│   ├── attendance-main.js         # 개인 근태 스크립트
│   └── attendance-depart.js       # 부서 근태 스크립트
└── calendar/
    ├── calendar-depart.js         # 부서 캘린더 스크립트
    └── calendar-team.js           # 팀 캘린더 스크립트
```

<br>

## 구현 기능

### 1. 근태관리 시스템

**REST API Controller**
- `AttendanceController.java` - 근태 화면 라우팅
- `TimeAndAttendanceRestController.java` - 출퇴근 기록 REST API
- `AttendanceStatsRestController.java` - 근태 통계 REST API

**프론트엔드 시각화 (D3.js)**
- `attendance-main.js` (26KB) - 개인 근태 대시보드
  - 주간/월간 근무 시간 통계
  - 출퇴근 기록 캘린더
  - 휴가 잔여일 표시
  
- `attendance-depart.js` (33KB) - 부서 근태 대시보드
  - D3.js 히트맵으로 부서원 근태 시각화
  - 월간 근무 시간 차트
  - 페이지네이션을 통한 데이터 탐색

**스케줄러**
- `quartz/` 패키지 - 자동 결근 처리 스케줄러

<br>

### 2. 캘린더 통합 시스템

**3-Layer 캘린더 구조**
- **개인 캘린더** (`users/`) - 개인 일정 관리
- **부서 캘린더** (`dept/`) - 부서 공통 일정
- **팀 캘린더** (`team/`) - 프로젝트 팀 일정

**FullCalendar 통합**
- `calendar-depart.js` (33KB) - 부서 캘린더 화면
- `calendar-team.js` (34KB) - 팀 캘린더 화면
- REST API와 FullCalendar 연동
- CRUD 기능 완전 구현

<br>

### 3. 휴가 관리 (`vacation/`)
- 휴가 신청 및 승인 워크플로우
- 잔여 휴가 계산 로직

<br>

### 4. 출장 관리 (`businesstrip/`)
- 출장 신청 및 상태 관리

<br>

### 5. AI 챗봇 (`vertex/`)
- Spring AI + Google Vertex AI 연동
- `ai-chat.jsp` - 챗봇 인터페이스

<br>

## REST API 구조 (예시)

### 근태 REST API

```java
// TimeAndAttendanceRestController.java
@RestController
@RequestMapping("/attendance")
public class TimeAndAttendanceRestController {
    // 출근 기록, 퇴근 기록, 근태 조회 등의 엔드포인트
}

// AttendanceStatsRestController.java
@RestController  
@RequestMapping("/attendance/stats")
public class AttendanceStatsRestController {
    // 주간 통계, 월간 통계, 부서 통계 등의 엔드포인트
}
```

### 캘린더 REST API

```java
// CalendarController.java
@Controller
@RequestMapping("/calendar")
public class CalendarController {
    // 부서 캘린더, 팀 캘린더 화면 라우팅
}
```

<br>

## 주요 기술 구현

### D3.js 근태 히트맵 시각화

`attendance-depart.js` (33KB)에서 구현:
- 부서원 × 날짜 매트릭스 히트맵
- 색상 그라데이션으로 근무 시간 표현
- 툴팁으로 상세 정보 제공

### FullCalendar 다중 소스 통합

`calendar-depart.js` / `calendar-team.js` 에서 구현:
- 개인/부서/팀 일정을 하나의 캘린더에 통합 표시
- 색상 코드로 일정 유형 구분
- 드래그 앤 드롭으로 일정 수정

### Quartz 스케줄러

`attendance/quartz/` 패키지:
- 매일 자정 결근 자동 처리
- 월말 통계 자동 생성

<br>

## 학습 및 성장 포인트

### 기술적 성장
- **REST API 설계**: Controller 계층 분리를 통한 관심사 분리
- **D3.js 시각화**: 대용량 데이터(100 여명 × 출근일) 렌더링 최적화 경험
- **FullCalendar**: 라이브러리 커스터마이징 및 REST API 연동
- **Spring AI**: Google Cloud Vertex AI 통합 경험

### 코드 품질
- 표준 패키지 구조 (`kr.or.ddit`) 준수
- Controller-Service-DAO 계층 분리
- RESTful URL 설계 원칙 적용

<br>

---

> 본 저장소는 팀 프로젝트에서 **제가 직접 작성한 코드만을 발췌**한 샘플입니다.  
> 실행 가능한 전체 프로젝트가 아니므로, 구현 예시는 예시 PNG, GIF를 참고해주세요.
