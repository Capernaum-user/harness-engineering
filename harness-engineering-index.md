---
category: "AI-Governance"
major_category: "AI-Governance"
middle_category: "Harness"
minor_category: "Index"
category_path: "AI-Governance/Harness/Index"
tags:
  - "harness"
  - "category/ai-governance"
  - "category/ai-governance/harness"
  - "category/ai-governance/harness/index"
created_at: "YYYY-MM-DD"
updated_at: "YYYY-MM-DD"
project: "harness"
type: "index"
tool: "manual"
status: "active"
visibility: "internal"
source_path: ""
related:
  - "00_Harness/ai-master-constitution.md"
  - "00_Harness/harness-change-history.md"
---

# <span style="color:#7c3aed">Harness Engineering Index</span>

Tags: #harness
관련 문서: [[00_Harness/ai-master-constitution.md]] · [[00_Harness/harness-change-history.md]] · {{SHARED_HUB}} · {{BOOT_PROTOCOL}}

## TL;DR

`00_Harness`는 여러 AI 에이전트(예: Claude, Gemini, Codex)가 공유하는 Vault 운영 규칙 묶음이다. 새 세션은 먼저 [[00_Harness/ai-master-constitution.md]]를 확인하고, 작업 유형에 맞는 세부 규칙으로 이동한다.

## 핵심 진입 순서

1. [[00_Harness/ai-master-constitution.md]] — 충돌 우선순위·보안·무결성 정본.
2. [[00_Harness/boot-manifest.md]] — 그 다음 무엇을 읽을지는 이 문서의 조건부 로드 표를 따른다.

## 파일 목록 (17개)

| 문서 | 역할 |
|---|---|
| [[00_Harness/ai-master-constitution.md]] | 최고 원칙, 보안·무결성 기준 (모든 설정보다 우선) |
| [[00_Harness/boot-manifest.md]] | 부팅 필수 문서 + 조건부 로드 표 |
| [[00_Harness/project-mission-context.md]] | AI 워크스페이스·핵심 프로젝트 맥락 |
| [[00_Harness/agent-role-definitions.md]] | 사용자·에이전트·로스터 역할 분담, 삼권분립(세션 역할) |
| [[00_Harness/agent-decision-rules.md]] | 자율 결정과 에스컬레이션 기준 |
| [[00_Harness/agent-prompt-standards.md]] | 프롬프트 구조와 응답 원칙 |
| [[00_Harness/agent-security-policy.md]] | 보안 금지 항목과 외부 전송 기준 |
| [[00_Harness/agent-systems-registry.md]] | 현행 가동 시스템 명부 |
| [[00_Harness/unattended-operations-policy.md]] | 무인·릴레이 실행 거버넌스 정본 |
| [[00_Harness/agent-workflow-diagram.md]] | 세션·협업·Vault 생명주기 흐름 |
| [[00_Harness/task-execution-protocol.md]] | 세션 부팅부터 완료 보고까지 실행 절차 |
| [[00_Harness/task-completion-standards.md]] | 완료·검수·품질 기준 정본 |
| [[00_Harness/session-continuity-protocol.md]] | 세션 연속성·메모리·인수인계 정본 |
| [[00_Harness/failure-recovery-protocol.md]] | 오류 등급과 복구 절차 |
| [[00_Harness/file-operations-rules.md]] | 파일명, 저장 위치, frontmatter, 동기화 정책 |
| [[00_Harness/harness-engineering-index.md]] | 하네스 전체 인덱스 (이 문서 계열의 진입점) |
| [[00_Harness/harness-change-history.md]] | 하네스 변경 이력 |

## 현행 운영 체계

세션 역할(삼권분립), 핸드오프 릴레이(ORDER/REPORT 파일 교환)·서브에이전트 로스터·훅 등 실제 가동 중인 시스템 목록은 [[00_Harness/agent-systems-registry.md]]. 무인·릴레이 실행 거버넌스는 [[00_Harness/unattended-operations-policy.md]].

보관 문서 원칙: 병합·폐기된 하네스 문서는 정본 문서에 내용을 병합한 뒤 `{{VAULT_ROOT}}\99_archive\` 아래 보관 폴더로 이동하고, 문서 상단에 보관 배너를 남긴다. 보관 문서는 현행 규칙의 출처로 사용하지 않는다.

## 관련 공통 기준

- {{SHARED_HUB}} — 공유 AI 설정 허브
- {{BOOT_PROTOCOL}} — 통합 부팅 프로토콜
- 전역 진입 파일(CLAUDE.md/GEMINI.md/AGENTS.md)

## 변경 이력 관리

이 인덱스를 포함한 하네스 문서의 변경 내역은 [[00_Harness/harness-change-history.md]]에 기록한다. 개별 문서 본문에는 개정 이력 서술을 남기지 않는다.
