# 기여 가이드 (Contributing)

knup-project(크누피/QuizFlow)에 기여해 주셔서 감사합니다 🎯
이 문서는 조직의 **모든 레포(frontend · backend · infra)** 에 공통으로 적용됩니다.

## 🌿 브랜치 전략

`main`은 항상 배포 가능한 상태를 유지합니다. 작업은 항상 브랜치를 따서 진행하고 PR로 머지합니다.

```
main                 # 프로덕션 (보호 브랜치, 직접 push 금지)
feat/<feature>       # 기능 개발   (예: feat/team-mode)
fix/<issue>          # 버그 수정   (예: fix/cors-preflight)
chore/<task>         # 설정·의존성 (예: chore/ci)
refactor/<area>      # 리팩토링
docs/<area>          # 문서
```

## 📝 커밋 컨벤션

[Conventional Commits](https://www.conventionalcommits.org/)를 따릅니다.

```
<type>(<scope>): <subject>

feat(session):   호스트 참가자 강퇴 API 추가
fix(auth):       CORS preflight(OPTIONS) 요청을 인증 검사에서 제외
chore(config):   Gemini 기본 모델을 gemini-2.5-flash-lite로 변경
refactor(session): SessionResponse 계약 정합성 정리
docs(infra):     배포 런북 링크 정리
```

| type | 용도 |
|---|---|
| `feat` | 기능 추가 |
| `fix` | 버그 수정 |
| `refactor` | 동작 변화 없는 구조 개선 |
| `chore` | 빌드·설정·의존성 |
| `docs` | 문서 |
| `style` | 포맷(코드 동작 무관) |
| `test` | 테스트 |

`scope`는 도메인을 권장합니다: `auth` · `quiz` · `session` · `ai` · `realtime` · `ui` · `infra` · `monitoring`.

## 🔀 Pull Request 흐름

1. 이슈를 먼저 등록하거나, 작업할 이슈를 고릅니다.
2. `main`에서 브랜치를 땁니다.
3. 작은 단위로 커밋하고, 로컬에서 빌드/린트가 통과하는지 확인합니다.
4. PR을 올리고 [PR 템플릿](.github/PULL_REQUEST_TEMPLATE.md)을 채웁니다. 관련 이슈를 `Closes #N`으로 연결합니다.
5. CI(빌드/테스트)가 초록불인지 확인합니다.
6. 리뷰 1인 이상 승인 후 **Squash & Merge**.

## ✅ 로컬 체크

| 레포 | 명령 |
|---|---|
| **frontend** | `pnpm install` → `pnpm lint` → `pnpm build` |
| **backend** | `./gradlew build` (테스트 포함) |
| **infra** | `tofu fmt -check` → `tofu validate` → `tofu plan` |

## 💬 커뮤니케이션

- 버그·기능 제안은 [이슈 템플릿](.github/ISSUE_TEMPLATE)으로 등록해 주세요.
- 자유로운 논의는 각 레포의 **Discussions** 탭을 이용합니다.

행복한 기여 되세요! 🙌
