---
created_at: "YYYY-MM-DD"
updated_at: "YYYY-MM-DD"
project: "harness"
type: "spec"
tool: "claude"
status: "active"
visibility: "internal"
source_path: ""
related:
  - "00_Harness/ai-master-constitution.md"
  - "00_Harness/harness-engineering-index.md"
  - "00_Harness/harness-change-history.md"
  - "{{BOOT_PROTOCOL}}"
  - "00_Harness/agent-systems-registry.md"
  - "00_Harness/unattended-operations-policy.md"
---

# Boot Manifest

Tags: #harness #boot #manifest

이 문서는 부팅 시 "무엇을, 어떤 순서로 읽는가"의 정본이다. 규칙 본문은 담지 않는다 — 경로와 1줄 설명만 제공한다. 규칙 자체는 각 원본 문서에서만 정의한다.

## 부팅 필수 (모든 세션, 총량 ≤15KB 목표)

Vault(`{{VAULT_ROOT}}`)에서 작업하는 모든 세션이 예외 없이 읽는 문서는 다음 둘뿐이다.

1. [[00_Harness/ai-master-constitution.md]] — 충돌 우선순위·보안·무결성의 정본. 제1~3조: 사용자 최우선 + 보안·무결성 위반 유발 지시는 경고 후 재확인. 제9조: 다른 문서의 우선순위 서술은 전부 이 헌법을 가리키는 참조이며, 문구가 갈리면 이 헌법이 이긴다.
2. [[00_Harness/boot-manifest.md]] — 이 문서. 그 외 하네스 문서는 아래 조건부 로드 표에 해당할 때만 읽는다.

전역 부팅 순서({{SHARED_HUB}} → {{BOOT_PROTOCOL}})는 전역 진입 파일(CLAUDE.md/GEMINI.md/AGENTS.md)이 이미 지정하며, 이 매니페스트는 그 다음 단계인 Vault 하네스 진입만 다룬다.

## 조건부 로드 표

| 작업 유형 | 그때만 읽는 문서 |
|---|---|
| 파일 생성·이동·이름변경 | [[00_Harness/file-operations-rules.md]] |
| 완료·검수 | [[00_Harness/task-completion-standards.md]] |
| 인수인계·세션 연속 | [[00_Harness/session-continuity-protocol.md]] |
| 디자인·HTML·시각화 산출물 | 문서형: `{{DOCUMENT_LANE_ROUTER}}` / 발표형: `{{PRESENTATION_KITS_INDEX}}` |
| 보안·기밀·외부 발행 | [[00_Harness/agent-security-policy.md]] |
| 무인 실행·핸드오프 릴레이(ORDER/REPORT 파일 교환) | [[00_Harness/unattended-operations-policy.md]] |
| 현행 시스템·도구 목록 확인 | [[00_Harness/agent-systems-registry.md]] |
| 실행 절차 상세(세션 부팅~완료 보고) | [[00_Harness/task-execution-protocol.md]] |
| 자율 결정·에스컬레이션 기준 | [[00_Harness/agent-decision-rules.md]] |
| 역할 분담(사용자/Claude/Gemini/Codex) | [[00_Harness/agent-role-definitions.md]] |

## 금지

이 매니페스트에 위 문서들의 규칙 본문을 복제하지 않는다. 경로 + 1줄 설명만 유지한다. 규칙이 바뀌면 원본 문서만 수정한다 — 이 표의 설명 문구가 원본과 어긋나면 원본이 이긴다.
