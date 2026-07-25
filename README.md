# Harness Engineering v2 — Pure Template

> 🔗 **소개 페이지(쇼케이스):** https://capernaum-user.github.io/harness-engineering/

멀티 AI 에이전트(Claude / Gemini / Codex 등)가 하나의 지식 저장소(Obsidian Vault 등) 위에서 **같은 규칙으로 협업**하게 만드는 거버넌스 프레임워크의 순수 템플릿입니다.

실제로 운영되던 하네스를 전수 감사(확정 결함 110건) → 전면 재설계 → 격리 세션 검증까지 거친 뒤, **사용자 고유 정보(이름·경로·프로젝트·URL)를 전부 플레이스홀더로 치환**해 추출했습니다. 그대로 복사해서 `{{...}}` 값만 채우면 자기 환경에 이식됩니다.

## 구조 — 17개 문서, 3계층

```
부팅 필수 (모든 세션이 읽는 단 2개, 합계 ≤15KB)
├─ ai-master-constitution.md      최고 원칙 — 사용자 주권·보안·무결성·정합화(제9조)
└─ boot-manifest.md               진입 순서 + 작업유형별 조건부 로드 표

조건부 로드 (작업 유형에 해당할 때만)
├─ agent-decision-rules.md        자율 결정 범위·에스컬레이션
├─ agent-role-definitions.md      세션 역할 기반 삼권분립(기획/구현/검증)
├─ agent-security-policy.md       비밀·PII·발행 게이트·봉인 경로
├─ agent-prompt-standards.md      프롬프트·보고 매체 표준
├─ agent-workflow-diagram.md      세션·협업 흐름 도식
├─ agent-systems-registry.md      현행 시스템 명부 (등재 형식)
├─ unattended-operations-policy.md 무인 실행 거버넌스 — "ORDER ≠ 사용자 승인"
├─ task-execution-protocol.md     부팅→실행→보고 절차
├─ task-completion-standards.md   완료의 정의·검수 체크리스트 (유일 정본)
├─ session-continuity-protocol.md 메모리·일지·인수인계 (유일 정본)
├─ failure-recovery-protocol.md   오류 등급·checkpoint 복원
├─ file-operations-rules.md       파일명·frontmatter·저장 위치·크기 예산
└─ project-mission-context.md     (템플릿) 조직·미션 컨텍스트 — 직접 채움

메타
├─ harness-engineering-index.md   전체 인덱스 (진입점)
└─ harness-change-history.md      하네스 자체의 변경 이력 장부
```

## 설계 원칙 6 — 이 템플릿이 지키는 것

1. **단일 정본 + 포인터** — 규칙마다 정본은 한 문서뿐. enum·서열·경로를 복제하지 않는다 (복제는 반드시 비동기 진화로 모순을 만든다).
2. **계층 사다리 단일화** — 충돌 우선순위는 헌법 제1~3조·제9조 한 곳: 사용자 지시 최우선, 단 보안·무결성 위반 소지는 경고 후 재확인.
3. **부팅 다이어트** — 필수 독서는 2문서. 나머지는 boot-manifest의 조건부 로드 표.
4. **단독/협업 이중 모드** — 모든 절차 문서에 "단독 세션"과 "릴레이·삼권분립 세션" 분기를 명시. 릴레이 ORDER는 사용자 승인이 아니다.
5. **세션 역할 기반 삼권분립** — 기획/구현/검증은 도구가 아니라 세션의 역할. 구현 세션은 자기 작업을 검증하지 않는다.
6. **신선도 내구성** — 문서에 날짜·개수를 박제하지 않는다. "비교해서 판정하라"는 절차문으로 쓴다. 하네스 수정은 change-history 기록과 한 몸.

## 적용 방법

1. 17개 `.md`를 Vault의 `00_Harness/` 폴더로 복사합니다.
2. [PLACEHOLDERS.md](PLACEHOLDERS.md)의 표를 보고 `{{...}}` 를 전부 자기 값으로 치환합니다 (에디터 전역 치환 권장).
3. `project-mission-context.md` 와 `agent-systems-registry.md` 를 자기 환경으로 채웁니다.
4. 각 AI의 전역 진입 파일(CLAUDE.md / GEMINI.md / AGENTS.md)에 아래 헤더를 넣어 하네스로 연결합니다:

```markdown
> 전역 부팅 후, Vault 하네스의 첫 읽기는 `00_Harness/ai-master-constitution.md`다.
> 이후 진입 순서는 `00_Harness/boot-manifest.md`를 따른다.
> 규칙을 여기에 새로 적거나 분기시키지 말 것 — 규칙은 항상 00_Harness/ 원본에서만 정의한다.
```

5. 첫 가동 후 `harness-change-history.md`에 "v2 템플릿 도입" 항목을 1건 기록하면 끝입니다.

## 검증 도구

- [verification-checklist.md](verification-checklist.md) — 하네스가 자기 규칙을 지키는지 깨끗한 세션이 실측 판정하는 체크 9종 (분기 1회 권장)
- [verification-methodology.md](verification-methodology.md) — 하네스를 크게 고칠 때 쓰는 4단계 검증 파이프라인 (감사 → 반박 → 커버리지 → 격리 검증)

## 운영하며 배운 것 (왜 이 모양인가)

- 규칙 붕괴의 공통 원인은 나쁜 규칙이 아니라 **같은 규칙의 복제 + 비동기 진화**였습니다. 그래서 포인터 체계입니다.
- "통합했다"는 기록만으로는 문서가 줄지 않습니다. 통합의 완료 기준에 **원본의 상태 변경(archived + 배너)**을 포함해야 합니다.
- 정본 문서가 자기 규칙을 어기는지(예: 필수 frontmatter 필드를 정본 자신이 누락)는 **작성자가 아닌 격리 세션**이 검사해야 잡힙니다.
- 5개 초과 파일 일괄 작업 전 git checkpoint 커밋은 선택이 아니라 관문입니다.

## License

MIT — see [LICENSE](LICENSE).
