<div align="center">

# 🎯 크누피 · QuizFlow <sub>(KNU-P)</sub>

### 경북대학교 실시간 퀴즈 플랫폼

호스트가 퀴즈를 만들면, 참가자는 **PIN 하나로 로그인 없이 입장**해<br/>
실시간으로 함께 풉니다. AI가 자료를 퀴즈로 바꿔주고, 끝나면 골드 시상까지.

<br/>

[![Frontend](https://img.shields.io/badge/▶_Live_Frontend-E60000?style=for-the-badge&logo=vercel&logoColor=white)](https://158-180-94-80.sslip.io)
[![Backend](https://img.shields.io/badge/⚙_Live_API-2C2C2C?style=for-the-badge&logo=springboot&logoColor=white)](https://144-24-92-17.sslip.io)

<br/>

![Next.js](https://img.shields.io/badge/Next.js_16-000000?style=flat-square&logo=next.js&logoColor=white)
![React](https://img.shields.io/badge/React_19-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_3.5-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Java](https://img.shields.io/badge/Java_21-007396?style=flat-square&logo=openjdk&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL_8-4479A1?style=flat-square&logo=mysql&logoColor=white)
![OpenTofu](https://img.shields.io/badge/OpenTofu-FFDA18?style=flat-square&logo=opentofu&logoColor=black)
![Oracle Cloud](https://img.shields.io/badge/OCI_Always_Free-F80000?style=flat-square&logo=oracle&logoColor=white)
![Gemini](https://img.shields.io/badge/Google_Gemini-8E75B2?style=flat-square&logo=googlegemini&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana_Cloud-F46800?style=flat-square&logo=grafana&logoColor=white)

</div>

---

## 📚 한눈에 보기

> **크누피**는 경북대학교(KNU) 강의실을 위한 Kahoot 스타일의 실시간 퀴즈 서비스입니다.
> 교수·강사(**호스트**)는 로그인 후 퀴즈를 만들고 세션을 열며, 학생(**참가자**)은 화면에 뜬
> **6자리 PIN**으로 로그인 없이 입장해 모바일에서 실시간으로 참여합니다.

| | 호스트 (교수·강사) | 참가자 (학생) |
|---|---|---|
| **로그인** | 필요 (세션 쿠키) | **불필요** (PIN 입장) |
| **플로우** | 퀴즈 생성 → 세션 개설 → 진행 → 결과 | PIN 입력 → 닉네임 → 실시간 참여 → 순위 |
| **화면** | PD 조종석(정답률·실시간 TOP5) | 타이머 링·4색 도형·셀러브레이션 |

---

## 🧩 레포지토리

조직은 **3개의 레포**로 구성된 마이크로 구조입니다. 각 레포는 독립 배포됩니다.

| 레포 | 설명 | 스택 | 상태 |
|---|---|---|---|
| **[frontend](https://github.com/knup-project/frontend)** | 호스트 대시보드 + 참가자 라이브 화면 (FSD 구조) | Next.js 16 · React 19 · TS · Tailwind v4 · Zustand · TanStack Query · STOMP | ![last](https://img.shields.io/github/last-commit/knup-project/frontend?style=flat-square&label=) ![issues](https://img.shields.io/github/issues/knup-project/frontend?style=flat-square) |
| **[backend](https://github.com/knup-project/backend)** | REST + WebSocket API, AI 퀴즈 생성, 게임 진행 엔진 | Spring Boot 3.5 · Java 21 · JPA · WebSocket · Gemini · Actuator | ![last](https://img.shields.io/github/last-commit/knup-project/backend?style=flat-square&label=) ![issues](https://img.shields.io/github/issues/knup-project/backend?style=flat-square) |
| **[infra](https://github.com/knup-project/infra)** | IaC, VM 부트스트랩, CI/CD, 모니터링 | OpenTofu · OCI · Docker Compose · nginx · GitHub Actions · Grafana | ![last](https://img.shields.io/github/last-commit/knup-project/infra?style=flat-square&label=) ![issues](https://img.shields.io/github/issues/knup-project/infra?style=flat-square) |

---

## 🏗️ 시스템 아키텍처

```mermaid
flowchart LR
    subgraph Client["🧑‍🏫 클라이언트"]
        H["호스트 데스크톱"]
        P["참가자 모바일"]
    end

    subgraph OCI["☁️ Oracle Cloud · Always Free · ap-chuncheon-1"]
        direction TB
        subgraph FE["Frontend VM · E2.1.Micro"]
            NF["nginx :443"] --> NX["Next.js 16 standalone<br/>(Docker)"]
        end
        subgraph BE["Backend VM · E2.1.Micro + 2GB swap"]
            NB["nginx :443"] --> SB["Spring Boot :8080<br/>(Docker)"]
            SB --> DB[("MySQL 8<br/>volume")]
            SB -. ":8081 actuator" .-> AL["Grafana Alloy"]
        end
    end

    subgraph SaaS["🌐 외부 SaaS (무료)"]
        GM["Google Gemini API"]
        GC["Grafana Cloud"]
    end

    H -- HTTPS --> NF
    P -- HTTPS --> NF
    H -- "REST /api/v1" --> NB
    H -- "WSS /ws (STOMP)" --> NB
    P -- "REST /api/v1" --> NB
    P -- "WSS /ws (STOMP)" --> NB
    SB -- "AI 생성·해설" --> GM
    AL -- "remote_write" --> GC
```

> 자세한 구현 아키텍처(엔드포인트 전수·데이터 모델·보안 제약·CI/CD)는
> **[📐 docs/ARCHITECTURE.md](https://github.com/knup-project/.github/blob/main/docs/ARCHITECTURE.md)** 참고.

---

## ✨ 핵심 기능

- 🎮 **실시간 게임 진행** — STOMP over WebSocket(SockJS fallback)로 문제·결과·리더보드 브로드캐스트
- 🔑 **무로그인 참가** — 6자리 PIN + 닉네임만으로 입장, 참가자 부담 0
- 🤖 **AI 퀴즈 생성** — 텍스트/PDF를 Google Gemini가 객관식·O/X·단답으로 자동 변환 + 해설
- 👥 **개인전 / 팀전** — 자동 팀 배정, 팀 리더보드
- 🏆 **라이브 리더보드** — 응답 속도 가중 점수, 골드 1등 시상·셀러브레이션 모션
- 🛡️ **호스트 운영** — 실시간 인원·강퇴(단건/일괄/전체)·유령 세션 자동 종료
- 📊 **관측성** — Actuator + Micrometer → Alloy → Grafana Cloud 대시보드/알림

---

## 🗓️ 개발 로드맵 (Gantt)

```mermaid
gantt
    title QuizFlow 개발 타임라인 (2026)
    dateFormat YYYY-MM-DD
    axisFormat %m/%d
    todayMarker off

    section 기획·설계
    제안서 · 아키텍처 설계        :done, plan, 2026-05-24, 3d

    section Backend
    도메인 · 인증 · 퀴즈 CRUD       :done, be1, 2026-05-24, 7d
    세션 · 실시간(STOMP)           :done, be2, 2026-05-31, 6d
    AI 퀴즈 생성(Gemini)           :done, be3, 2026-06-04, 4d
    모니터링 · 안정화              :done, be4, 2026-06-08, 5d

    section Frontend
    기반 · 디자인 시스템           :done, fe1, 2026-05-24, 7d
    호스트 / 참가자 플로우         :done, fe2, 2026-05-31, 7d
    실시간 연동 · UX 개선          :done, fe3, 2026-06-06, 6d

    section Infra
    OCI VM · 네트워크(OpenTofu)    :done, inf1, 2026-05-27, 5d
    Docker · CI/CD · HTTPS         :done, inf2, 2026-06-01, 4d
    모니터링(Grafana Cloud)        :done, inf3, 2026-06-05, 4d

    section 출시
    통합 · 안정화 · 데모           :active, rel, 2026-06-10, 3d
```

전체 마일스톤·백로그는 **[📋 docs/ROADMAP.md](https://github.com/knup-project/.github/blob/main/docs/ROADMAP.md)** 에 정리되어 있습니다.

---

## 👥 팀 — knup-project

> 경북대학교 오픈소스 SW 팀 프로젝트 팀입니다.

| 역할 | 멤버 | 담당 |
|---|---|---|
| 🎨 **Frontend Lead** | [@eunwoo-levi](https://github.com/eunwoo-levi) | Next.js 앱, 디자인 시스템, 실시간 UX |
| ⚙️ **Backend Lead** | [@hanseung-yeon](https://github.com/hanseung-yeon) | API·게임 엔진, 인증, AI 연동 |
| ☁️ **Infra Lead** | [@JinVibe](https://github.com/JinVibe) | OCI/OpenTofu, CI/CD, 모니터링 |
| 🛠️ **Infra · DevOps · QA** | [@lsmin3388](https://github.com/lsmin3388) | 배포 자동화, 크로스레포 계약 정합, 품질 보증 |

---

## 🛠️ 기술 스택

<div align="center">

**Frontend**<br/>
![Next.js](https://img.shields.io/badge/Next.js_16-000?logo=next.js) ![React](https://img.shields.io/badge/React_19-20232A?logo=react&logoColor=61DAFB) ![TS](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white) ![Tailwind](https://img.shields.io/badge/Tailwind_v4-06B6D4?logo=tailwindcss&logoColor=white) ![Zustand](https://img.shields.io/badge/Zustand_5-433E38?logo=react) ![TanStack](https://img.shields.io/badge/TanStack_Query-FF4154?logo=reactquery&logoColor=white)

**Backend**<br/>
![Spring](https://img.shields.io/badge/Spring_Boot_3.5-6DB33F?logo=springboot&logoColor=white) ![Java](https://img.shields.io/badge/Java_21-007396?logo=openjdk&logoColor=white) ![JPA](https://img.shields.io/badge/Spring_Data_JPA-6DB33F?logo=spring&logoColor=white) ![MySQL](https://img.shields.io/badge/MySQL_8-4479A1?logo=mysql&logoColor=white) ![WebSocket](https://img.shields.io/badge/STOMP_/_WebSocket-010101?logo=socketdotio&logoColor=white) ![Gradle](https://img.shields.io/badge/Gradle-02303A?logo=gradle&logoColor=white)

**Infra & Ops**<br/>
![OpenTofu](https://img.shields.io/badge/OpenTofu-FFDA18?logo=opentofu&logoColor=black) ![OCI](https://img.shields.io/badge/Oracle_Cloud-F80000?logo=oracle&logoColor=white) ![Docker](https://img.shields.io/badge/Docker_Compose-2496ED?logo=docker&logoColor=white) ![nginx](https://img.shields.io/badge/nginx-009639?logo=nginx&logoColor=white) ![Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?logo=githubactions&logoColor=white) ![Grafana](https://img.shields.io/badge/Grafana_Cloud-F46800?logo=grafana&logoColor=white)

</div>

---

## 🤝 기여하기

오픈소스 팀 프로젝트입니다. 컨벤션과 PR 흐름은 조직 공통 문서를 따릅니다.

- 📜 [기여 가이드 (CONTRIBUTING)](https://github.com/knup-project/.github/blob/main/CONTRIBUTING.md)
- 🤲 [행동 강령 (Code of Conduct)](https://github.com/knup-project/.github/blob/main/CODE_OF_CONDUCT.md)
- 🔐 [보안 정책 (Security)](https://github.com/knup-project/.github/blob/main/SECURITY.md)

> 커밋은 [Conventional Commits](https://www.conventionalcommits.org/), 브랜치는 `feat/*` · `fix/*` · `chore/*` · `refactor/*` · `docs/*`, 머지는 PR 기반.

<div align="center">
<br/>

**Made with ❤️ & ☕ by the knup-project team · 경북대학교 2026**

</div>
