---
category: "AI-Governance"
major_category: "AI-Governance"
middle_category: "Harness"
minor_category: "Log"
category_path: "AI-Governance/Harness/Log"
tags:
  - "harness"
  - "changelog"
  - "category/ai-governance"
  - "category/ai-governance/harness"
  - "category/ai-governance/harness/log"
created_at: "YYYY-MM-DD"
updated_at: "YYYY-MM-DD"
project: "harness"
type: "log"
tool: "claude|gemini|chatgpt|codex|manual|auto-generated"
status: "active"
visibility: "internal"
source_path: ""
related:
  - "00_Harness/harness-engineering-index.md"
  - "00_Harness/file-operations-rules.md"
---

# Harness Change History

Tags: #harness #changelog
관련 문서: [[00_Harness/harness-engineering-index.md]] · [[00_Harness/file-operations-rules.md]]

## TL;DR

이 문서는 `00_Harness` 규칙 자체의 변경 이력을 기록한다. 세부 작업 로그는 각 AI work diary에 남기고, 하네스 규칙의 구조·정책 변경만 이 문서에 요약한다.

## 변경 기록 원칙

- 하네스 규칙의 의미가 바뀌면 이 문서에 날짜, 변경 주체, 변경 범위, 이유를 기록한다.
- 단순 오탈자 수정은 work diary에만 기록할 수 있다.
- 보안, 동기화, 파일명, frontmatter, 역할 분담 변경은 반드시 이 문서에 남긴다.
- 이력이 정지되지 않도록 한다: 규칙 문서를 실제로 바꾼 세션이 같은 세션 안에서 이 문서에 항목을 추가한다.
- 사후에 소급 기록할 때는 근거(커밋 해시, 파일 mtime 등)를 명기하고, 근거가 없는 부분은 추정 단정 대신 "검증 불가"로 남긴다.
- 보관(archive)된 규칙 문서는 `{{VAULT_ROOT}}\99_archive\` 아래에 `status: archived` frontmatter와 상단 보관 배너를 붙여 둔다.
- 이 문서가 40 KB(분할 기준)에 접근하면 오래된 항목을 요약·롤오버하는 계획을 세운다.

## 항목 형식 템플릿

새 항목은 시간순(과거→최신)으로 아래 형식을 따른다. 필요 없는 필드는 생략할 수 있으나, 날짜·실행 주체·변경 이유·변경 범위·검증은 가능한 한 항상 채운다.

```markdown
## YYYY-MM-DD <변경 제목 (한 줄 요약)>

- order_id: <핸드오프 릴레이(ORDER/REPORT 파일 교환)로 발주된 작업이면 ORDER 식별자, 아니면 생략>
- 실행 주체: <Claude | Gemini | Codex | 사용자 | ...>
- 변경 이유: <어떤 문제·모순·요구가 이 변경을 촉발했는지>
- 변경 범위:
  - `<수정한 문서 경로 1>`: <무엇을 어떻게 바꿨는지>
  - `<수정한 문서 경로 2>`: <...>
- 검증: <grep/파일 수/크기 등 실측 확인 방법과 결과>
- 미처리 `[확인필요]`: <남은 항목, 없으면 "없음">
```

## 예시 항목 (placeholder)

## YYYY-MM-DD 하네스 엔지니어링 v1 생성

- 실행 주체: {{AGENT_NAME}}
- 생성 위치: `{{VAULT_ROOT}}\00_Harness`
- 변경 이유: 여러 AI 에이전트(Claude, Gemini, Codex 등)가 동일한 Vault 운영 기준을 참조하도록 분리된 하네스 구성이 필요했다.
- 변경 범위:
  - `00_Harness/` 운영 규칙 문서 {{N}}개 신설
  - 각 문서에 YAML frontmatter(visibility 포함)와 표준 tags 추가
  - 주요 문서 간 Obsidian 내부 링크 추가
- 검증: `00_Harness\*.md` 파일 수 {{N}}개 확인, frontmatter 필수 필드 누락 0건.
- 미처리 `[확인필요]`: 없음.

## 변경 요약

- 하네스 자체의 추적 가능한 changelog를 추가했다.
