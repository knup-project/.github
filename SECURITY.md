# 보안 정책 (Security Policy)

## 지원 버전

크누피(QuizFlow)는 활발히 개발 중인 프로젝트로, 항상 **최신 `main` 브랜치**를 기준으로 보안 수정을 제공합니다.

| 버전 | 지원 |
|---|---|
| `main` (latest) | ✅ |
| 이전 태그 릴리스 | ⚠️ best-effort |

## 취약점 제보

보안 취약점을 발견하셨다면 **공개 이슈로 올리지 말고** 아래로 비공개 제보해 주세요.

- 🔒 GitHub **Security Advisories** (각 레포 → Security → Report a vulnerability)
- 📧 메일: `loonaticvibe2.11@gmail.com`

제보 시 다음을 포함해 주시면 빠르게 대응할 수 있습니다.

- 영향받는 레포/엔드포인트/컴포넌트
- 재현 절차 (PoC)
- 예상 영향 범위

가능한 한 **72시간 이내**에 1차 회신드립니다.

## 알려진 보안 제약 (설계상)

본 프로젝트는 학습용 팀 프로젝트로, 다음 제약을 문서화해 둡니다(상세: [docs/ARCHITECTURE.md §11](docs/ARCHITECTURE.md)).

- **참가자 무로그인 설계** → WebSocket 채널 인증 없음(의도된 설계)
- **HttpSession 인메모리** → 백엔드 재배포 시 로그인 세션 소멸
- **cross-domain 쿠키**(프론트/백엔드 도메인 분리) → 일부 브라우저의 third-party 쿠키 차단 영향

민감정보(DB·Gemini·OCI 키)는 레포에 커밋하지 않으며, VM `/opt/knup/.env`와 GitHub Actions Secrets로만 주입합니다.
