---
category: "AI-Governance"
major_category: "AI-Governance"
middle_category: "Harness"
minor_category: "Workflow"
category_path: "AI-Governance/Harness/Workflow"
tags:
  - "harness"
  - "workflow"
  - "category/ai-governance"
  - "category/ai-governance/harness"
  - "category/ai-governance/harness/workflow"
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
  - "00_Harness/task-execution-protocol.md"
  - "00_Harness/session-continuity-protocol.md"
  - "00_Harness/unattended-operations-policy.md"
  - "00_Harness/agent-role-definitions.md"
---
# 워크플로우 다이어그램 (Workflow Diagram)
# Harness Engineering v1

Tags: #harness #workflow
관련 문서: [[00_Harness/harness-engineering-index.md]] · [[00_Harness/harness-change-history.md]] · {{SHARED_HUB}} · {{BOOT_PROTOCOL}} · [[00_Harness/task-execution-protocol.md]]

AI 에이전트의 주요 작업 흐름을 다이어그램으로 표현한다.




## 1. 세션 전체 흐름

> 진입 순서는 전역 진입 파일(CLAUDE.md/GEMINI.md/AGENTS.md)의 첫읽기 문구와 문자 그대로 일치한다: 전역 설정 → 허브 → 통합 부팅 프로토콜 → (Vault 진입 시) 헌법 첫 읽기 → boot-manifest 조건부 로드. 헌법을 허브보다 앞에 그리지 않는다.

```
┌─────────────────────────────────────────────────────────────┐
│                     세션 시작                                │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
                    ┌───────────────┐
                    │  전역 설정 로드 │  ← CLAUDE.md / GEMINI.md / AGENTS.md
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │  Shared Hub   │  ← {{SHARED_HUB}}
                    │  확인         │
                    └───────┬───────┘
                            │
                            ▼
                    ┌──────────────────────┐
                    │  통합 부팅 프로토콜  │  ← {{BOOT_PROTOCOL}}
                    └─────────┬────────────┘
                              │
                              ▼
                    ┌──────────────────────┐
                    │  (Vault 진입 시)     │  ← ai-master-constitution.md
                    │  헌법 첫 읽기        │     (충돌 우선순위·보안·무결성 정본)
                    └─────────┬────────────┘
                              │
                              ▼
                    ┌──────────────────────┐
                    │  boot-manifest       │  ← 조건부 로드 표에 따라
                    │  조건부 로드         │     00_Harness/ 세부 문서 선택
                    └─────────┬────────────┘
                              │
                              ▼
                    ┌───────────────┐
                    │  Vault 검색   │  ← 관련 문서 탐색
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │  작업 실행    │
                    └───────┬───────┘
                            │
                    ┌───────┴───────┐
                    │               │
                    ▼               ▼
            ┌──────────┐    ┌──────────────┐
            │  성공     │    │  실패/오류    │
            └────┬─────┘    └──────┬───────┘
                 │                 │
                 │                 ▼
                 │          ┌──────────────────┐
                 │          │ failure-recovery │
                 │          │  -protocol.md    │
                 │          └──────┬───────────┘
                 │                 │
                 └────────┬────────┘
                          │
                          ▼
                  ┌───────────────┐
                  │  품질 검증    │  ← task-completion-standards.md
                  │  체크리스트   │
                  └───────┬───────┘
                          │
                    ┌─────┴──────┐
                    │            │
                    ▼            ▼
             ┌──────────┐  ┌──────────────┐
             │  통과     │  │  미통과       │
             └────┬─────┘  │  → 수정 후   │
                  │        │    재검증     │
                  │        └──────────────┘
                  ▼
          ┌────────────────┐
          │  완료 보고      │  ← task-completion-standards.md 형식
          └───────┬────────┘
                  │
                  ▼
          ┌────────────────┐
          │  Work Diary    │  ← 03_ai_notes/work-logs/
          │  업데이트       │
          └────────────────┘
```

---

## 2. 에스컬레이션 결정 트리

```
작업 요청 수신
      │
      ▼
 파괴적 작업인가?
 (삭제, push, 외부전송)
      │
   ┌──┴──┐
  Yes    No
   │      │
   ▼      ▼
사용자  보안 위반인가?
확인    (시크릿, PII)
필수      │
        ┌─┴─┐
       Yes  No
        │    │
        ▼    ▼
     즉시   50개 이상
     중단   파일 수정?
     보고      │
            ┌──┴──┐
           Yes    No
            │      │
            ▼      ▼
          사용자  자율 진행
          확인    가능
```

---

## 3. AI 협업 흐름

### 3a. 핸드오프 릴레이 흐름 — ORDER/REPORT 파일 교환 (1차 경로)

```
기획(Planner) 세션
     │  발주 스킬 → ORDER 작성
     ▼
{{RELAY_ROOT}}/orders/  ────────────────┐
                                        │
                       ┌────────────────┴────────────────┐
                       │                                   │
                       ▼                                   ▼
              (선택) 교차검증 스킬                 수행 스킬
              외부 AI 교차검증 릴레이              워커(Creator) 세션이 클레임
              (2레인, 예: Codex·Gemini)                   │
                       │                                   ▼
                       │                          구현 (자가 테스트 없이)
                       │                                   │
                       │                                   ▼
                       │                          {{RELAY_ROOT}}/reports/  (REPORT)
                       │                                   │
                       └──────────────┬────────────────────┘
                                       ▼
                              검토 스킬 (기획 세션)
                              3중 게이트: echo · evidence · boundary
                                       │
                          ┌────────────┴────────────┐
                          ▼                          ▼
                        수락                    반려 → ORDER R(n+1) 재발주
                          │
                          ▼
                  {{RELAY_ROOT}}/archive/ + 사용자 보고

[공통] 무인 실행 거버넌스: [[00_Harness/unattended-operations-policy.md]]
에스컬레이션 발생 시 킬스위치: {{RELAY_ROOT}}/STOP-<작업ID>
```

### 3b. 레거시/보조 흐름 — 도구 고정 3-AI 분업

> 아래는 초기 설계 당시의 도구 고정 분업 예시다. 현재는 삼권분립이 **세션 역할**이지 도구 이름에 고정되지 않는다([[00_Harness/agent-role-definitions.md]] "삼권분립(세션 역할)"). 이 흐름은 참고용 레거시 패턴으로 남긴다 — 신규 협업은 3a(릴레이)를 우선한다.

```
사용자 요청
     │
     ▼
 ┌─────────┐     기획·리서치      ┌─────────┐
 │ Claude  │ ──────────────────▶ │ Gemini  │
 │         │ ◀────────────────── │         │
 │ 문서·   │    초안·조사 결과    │ 리서치· │
 │ 자동화  │                     │ 기획    │
 └────┬────┘                     └─────────┘
      │
      │  코드 구현 위임
      ▼
 ┌─────────┐
 │ Codex   │
 │         │
 │ 코드·   │
 │ 테스트  │
 └────┬────┘
      │
      │ 결과물 반환
      ▼
 ┌─────────┐
 │ Claude  │
 │         │  ──▶  Vault 저장
 │ 통합·   │  ──▶  Work Diary
 │ 정리    │  ──▶  사용자 보고
 └─────────┘

[공통]
각 AI는 작업 후 자신의 Work Diary에 기록
인수인계: session-continuity-protocol.md 형식
```

---

## 4. 멀티 AI 개발 프로젝트 워크플로우 (일반)

> 과거 종료된 개발 프로젝트 경험을 일반화한 템플릿. 신규 개발 프로젝트에 그대로 적용한다.

```
요구사항 정의 (사용자)
       │
       ▼
  리서치·기획 (Gemini)
  - 기술 옵션 조사
  - 아키텍처 검토
       │
       ▼
  설계 문서 (Claude)
  - spec 문서 → Vault
  - API 설계 문서
       │
       ▼
  코드 구현 (Codex)
  - Python / FastAPI / React
  - 테스트 작성
       │
       ▼
  코드 리뷰 (Claude + 사용자)
       │
       ▼
  통합·배포 준비 (Codex)
       │
       ▼
  최종 문서화 (Claude)
  - 완료 보고 → Vault
```

---

## 5. Vault 파일 생명주기

```
생성 (draft)
  └─▶ 작업 중 (active)
        └─▶ 완료 (done)
              └─▶ 보관 (archived) → 99_archive/
```

```
00_inbox/ (미분류)
    │
    ▼ 분류
01_projects/<project>/
    │
    ▼ 완료
99_archive/
```

---

## 6. 보안 처리 흐름

```
시크릿 감지
     │
     ▼
 즉시 중단 ──▶ 사용자 알림 (P0)
                    │
                    ▼
             파일에서 제거
                    │
              ┌─────┴──────┐
              │            │
              ▼            ▼
        git에 없음    git history에 있음
              │            │
              ▼            ▼
          완료 처리   git-filter-repo
                      사용 안내
```
