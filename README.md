# 프로젝트 분석 문서

## 📋 프로젝트 개요

**프로젝트명**: TeamHub  
**프로젝트 유형**: 풀스택 웹 애플리케이션 (React 프론트엔드 + Spring Boot 백엔드)  
**개발 목적**: 숙소/레저 예약, 공지/FAQ/QnA, 리뷰 관리 등 통합 예약/커뮤니티 서비스  
**구성 폴더**: `teamhub`(프론트엔드), `th`(백엔드), `thImg`(이미지 에셋)

---

## 🏗️ 기술 스택

### 프론트엔드
- **프레임워크**: React (Create React App 기반)
- **언어**: JavaScript (ES6+)
- **스타일링**: CSS 모듈(일반 CSS 파일 구성)
- **번들링/개발서버**: CRA 내장(Webpack)

### 백엔드
- **플랫폼**: Spring Boot
- **언어**: Java
- **빌드 도구**: Gradle
- **보안/인증**: Spring Security, JWT
- **기타**: 이메일 전송, SMS, 결제(PortOne) 연계 구성 흔적

### 공통/환경
- **버전관리**: Git, GitHub
- **이미지 에셋**: `thImg/` 및 `teamhub/public/img/` 내 정적 리소스

---

## 📁 프로젝트 구조

```
TeamHub/
├── teamhub/                       # 프론트엔드 (React)
│   ├── public/                    # 정적 리소스(파비콘/이미지/manifest 등)
│   └── src/                       # React 소스
│       ├── Activity/              # 액티비티(레저) 목록/상세
│       ├── components/            # 공통 UI 컴포넌트
│       ├── css/                   # 화면별 스타일
│       ├── data/                  # 정적 데이터(RoomData, faqData 등)
│       ├── faq/, notice/, qna/    # FAQ/공지/QnA 화면
│       ├── home/                  # 메인/헤더/로그인 등 홈 관련 컴포넌트
│       ├── mypage/                # 마이페이지/리뷰/예약 관련 화면 및 api 래퍼
│       ├── pages/                 # 레저/결제/객실 상세 등 주요 페이지
│       └── room/                  # 객실 목록/결제 화면
├── th/                            # 백엔드 (Spring Boot)
│   ├── build.gradle, settings.gradle, gradlew*  # Gradle 빌드 스크립트
│   └── src/main/java/kr/co/th/
│       ├── Controller/            # 도메인별 컨트롤러(Notice/QnA/Review/Room 등)
│       ├── Repository/            # JPA Repository 레이어
│       ├── Service/               # 서비스 레이어(비즈니스 로직)
│       ├── config/                # 보안/JWT/웹 설정/PortOne 설정
│       ├── dto/, vo/              # 전송/도메인 객체
│       └── util/                  # JWT 필터/유틸리티
└── thImg/                         # 이미지 에셋(예: 객실/액티비티 이미지)
```

---

## 🔑 주요 기능

### 1. 예약/객실
- 객실 목록/상세 조회(`teamhub/src/pages/RoomIntro.js`, `RoomDetail.js`)
- 객실 결제 흐름 화면(`teamhub/src/room/RoomPay.js`)

### 2. 레저(액티비티)
- 레저 목록/상세/예약 화면(`Activity/*`, `pages/Leisure*`, `components/WaterLeisureReservation.js`)

### 3. 공지/FAQ/QnA
- 공지 목록/상세/수정/등록(`notice/*`)
- FAQ 목록/상세/수정/등록(`faq/*`)
- QnA 목록/상세/관리/작성(`qna/*`)

### 4. 리뷰
- 마이페이지 리뷰 목록/작성/수정(`mypage/*`, `components/RoomCard.js` 등 연계)

### 5. 결제
- 프론트 결제 페이지(`pages/PaymentPage.js`)
- 백엔드 `PortOneService`, `PortOneConfig` 등으로 결제 연계 구성 흔적

### 6. 인증/보안
- 백엔드 `SecurityConfig`, `JwtAuthenticationFilter`, `JwtUtil` 기반 JWT 인증

---

## 🔌 백엔드 API 개요(코드 기준 유추)

> 실제 엔드포인트는 컨트롤러 매핑에 따르며, 아래는 파일명 기반의 기능 범주입니다.

- `Controller/*`
  - `NoticeController`: 공지 CRUD
  - `FaqController`: FAQ CRUD
  - `QnAController`: QnA CRUD/관리
  - `ReviewController`: 리뷰 등록/조회
  - `ReservationController`: 예약 관련 처리
  - `RoomController`: 객실 데이터 제공
  - `WaterReservationController`: 수상 레저 예약
  - `UserController`: 사용자 관련 기능
  - `ChatController`, `SmsController`: 채팅/SMS 전송 등 부가 기능
- `Service/*`: 각 도메인 서비스 로직, `PortOneService`(결제), `EmailService`, `SmsService` 포함
- `Repository/*`: 엔티티별 JPA Repository
- `config/*`: Spring Security, JWT 필터/유틸 설정, CORS/웹 설정

---

## 🖥️ 실행 방법

### 프론트엔드(teamhub)
1. 의존성 설치: `npm install`
2. 개발 서버 실행: `npm start`
3. 접속: `http://localhost:3000`

### 백엔드(th)
1. Gradle 빌드: `./gradlew build` (Windows: `gradlew.bat build`)
2. 실행: `./gradlew bootRun` (Windows: `gradlew.bat bootRun`)
3. 기본 포트(예상): `http://localhost:8080`  
   - 보안/토큰이 필요한 API는 JWT 헤더 설정 필요

---

## 🌐 배포/환경
- 프론트는 CRA 빌드(`npm run build`) 산출물 배포
- 백엔드는 Spring Boot JAR 배포 또는 서버/컨테이너 실행
- 환경 변수/시크릿: 프론트 `.env`(미추적), 백엔드 `application.properties` 관리

---

## 📦 에셋 리소스
- 프론트 정적 리소스: `teamhub/public/` (파비콘/이미지/manifest 등)
- 공용 이미지: `thImg/` (객실/액티비티 사진 등)

---

## 🚧 현재 저장소 상태(요약)
- 루트 `public/`, `src/`는 정리(삭제) 완료 — 프론트는 `teamhub/` 하위만 사용
- 백엔드 `th/`와 에셋 `thImg/` 포함
- `.gitignore` 설정으로 빌드산출물/업로드/캐시 제외

---

## 🔄 향후 개선 사항(권장)
1. **API 스키마 문서화**: Swagger/OpenAPI 추가로 프론트-백엔드 인터페이스 명세화
2. **에러 처리 일원화**: 백엔드 예외/응답 포맷 표준화, 프론트 에러 UI 통일
3. **상태 관리 도입**: React Query/Redux 등으로 데이터 일관성 향상
4. **빌드/배포 파이프라인**: GitHub Actions로 CI/CD 구축
5. **보안 강화**: CORS/토큰 만료/리프레시 토큰 정책 명확화, 시크릿 분리
6. **이미지/정적 자원 관리**: CDN 적용 및 이미지 최적화

---

## 📄 라이선스 및 기타
- **버전 관리**: Git/GitHub (`main`)  
- **저장소**: `https://github.com/jay0803/TeamHub.git`

---

## 📞 연락처 및 참고
- 프로젝트/팀 정보는 추후 `README.md` 및 문서에 통합 권장

---

**문서 생성일**: 2025년 11월 6일  
**마지막 업데이트**: 저장소 분석 기준


