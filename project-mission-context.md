---
category: "AI-Governance"
major_category: "AI-Governance"
middle_category: "Harness"
minor_category: "Spec"
category_path: "AI-Governance/Harness/Spec"
tags:
  - "harness"
  - "mission"
  - "category/ai-governance"
  - "category/ai-governance/harness"
  - "category/ai-governance/harness/spec"
created_at: "YYYY-MM-DD"
updated_at: "YYYY-MM-DD"
project: "harness"
type: "spec"
tool: "claude"
status: "active"
visibility: "internal"
source_path: ""
related:
  - "00_Harness/harness-engineering-index.md"
  - "00_Harness/harness-change-history.md"
  - "00_Harness/agent-role-definitions.md"
---
# 프로젝트 미션 (Project Mission)
# Harness Engineering v1

Tags: #harness #mission
관련 문서: [[00_Harness/harness-engineering-index.md]] · [[00_Harness/harness-change-history.md]] · [[00_Harness/agent-role-definitions.md]] · {{SHARED_HUB}} · {{BOOT_PROTOCOL}}

모든 AI가 공유하는 프로젝트 목표와 컨텍스트.
작업 우선순위 판단 및 방향 설정의 기준이 된다.

<!-- 작성 안내: 이 문서는 "채워 넣는 템플릿"이다. {{...}} 자리를 자신의 환경 값으로
     교체하고, 불필요한 소절은 삭제한다. 섹션 구조(종료 프로젝트 → 1차 미션 →
     협업 인프라 → 조직 컨텍스트 → 기술 역량)는 유지할 것을 권장한다. -->

## [종료] 과거 프로젝트: {{LEGACY_PROJECT_NAME}}

> **[YYYY-MM-DD 기준 종료]** {{LEGACY_PROJECT_NAME}}({{LEGACY_PROJECT_DESCRIPTION}})는 완료된 과거 프로젝트다.
> 현재 활성 미션이 아님. AI는 이 프로젝트를 현재 작업 컨텍스트에서 우선 추천하지 않는다.

<!-- 작성 안내: 종료된 프로젝트가 없으면 이 섹션을 삭제한다. 종료 프로젝트를 명시해 두면
     AI가 낡은 컨텍스트를 현재 작업으로 오인하는 것을 막을 수 있다. -->

---

## 1차 미션: {{PRIMARY_MISSION_TITLE}}

**목표:** {{PRIMARY_MISSION_GOAL}}
<!-- 예: 복수의 AI 에이전트(예: Claude, Gemini, Codex)가 일관된 기준으로 협업하는 환경 유지 -->

**핵심 도구:**
- Obsidian Vault (`{{VAULT_ROOT}}`) — 최종 문서 저장소
- 이 하네스 시스템 (`{{VAULT_ROOT}}\00_Harness\`) — AI 행동 기준
- 공통 설정 허브({{SHARED_HUB}}) — AI별 전역 설정 연결점

**성공 기준:**
- {{SUCCESS_CRITERION_1}}
  <!-- 예: 모든 AI가 동일한 파일명·frontmatter 규칙으로 Vault에 문서 생성 -->
- {{SUCCESS_CRITERION_2}}
  <!-- 예: 각 AI의 work diary가 체계적으로 기록됨 -->
- {{SUCCESS_CRITERION_3}}
  <!-- 예: 세션 간 컨텍스트가 하네스를 통해 유지됨 -->

### 협업 인프라

<!-- 작성 안내: 미션이 어떤 협업 인프라 위에서 돌아가는지 선언하고, 각 인프라가
     어느 프로젝트에 귀속되는지 명시한다. 아래는 일반화된 두 가지 예시 유형이다. -->

1차 미션은 다음 협업 인프라 위에서 돌아간다:

- **서브에이전트 로스터** — {{AGENT_ROSTER_DESCRIPTION}} (예: chief-director, researcher, builder-1 … 같은 역할 lane 구성; 정본: `{{AGENT_ROSTER_DOC}}`)
- **핸드오프 릴레이(ORDER/REPORT 파일 교환)** — 기획 세션→워커 세션 비동기 위임 (정본: `{{HANDOFF_PROTOCOL_DOC}}`)

각 인프라의 프로젝트 귀속은 명시적으로 적는다: {{INFRA_OWNERSHIP_NOTE}}
<!-- 예: "둘 다 조직 프로젝트 산하가 아니라 에이전트 스튜디오 프로젝트에 귀속된다." -->

---

## 조직 컨텍스트: {{ORG_NAME}}

**유형:** {{ORG_TYPE}}
<!-- 예: 창업 준비 단계 스타트업 / 1인 사업자 / 사내 팀 -->

**개발 정책:** {{ORG_DEV_POLICY}}
<!-- 예: 특정 언어 우선 정책(시스템/서비스/CLI/자동화 → 1순위 언어 지정) -->

**기밀 처리:** 회사 기밀 정보는 `{{CONFIDENTIAL_DIR}}` 폴더의 별도 문서 기준으로 처리

---

## 현재 기술 역량 (사용자 기준)

<!-- 작성 안내: AI가 설명 수준·자동화 범위를 조절할 수 있도록 사용자의 기술 스택과
     숙련도를 표로 적는다. 행은 자유롭게 추가·삭제한다. -->

| 분야 | 수준 |
|------|------|
| {{SKILL_AREA_1}} | {{SKILL_LEVEL_1}} <!-- 예: 주력, 실무 가능 --> |
| {{SKILL_AREA_2}} | {{SKILL_LEVEL_2}} <!-- 예: 학습 중 --> |
| {{SKILL_AREA_3}} | {{SKILL_LEVEL_3}} |

자격·인증: {{CERTIFICATIONS}}

---

*최초 작성: YYYY-MM-DD*
