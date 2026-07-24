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
  - "00_Harness/harness-engineering-index.md"
  - "00_Harness/agent-role-definitions.md"
  - "00_Harness/task-execution-protocol.md"
---
# 세션 연속성 프로토콜 (Session Continuity Protocol)
# Harness Engineering v1

Tags: #harness #memory #handoff
관련 문서: [[00_Harness/harness-engineering-index.md]] · [[00_Harness/agent-role-definitions.md]] · [[00_Harness/task-execution-protocol.md]]

세션 간 정보를 유지하고 AI 간 작업을 인수인계하는 방법을 정의한다.

> 메모리 규칙 문서 + 인수인계 프로토콜 문서 두 파일을 통합한 문서다.

**이 문서가 세션 연속성·메모리·인수인계의 유일 정본이다.** 통합 전 구 문서는 `{{VAULT_ROOT}}\99_archive\` 에 배너와 함께 보관한다.

## 1. 에이전트 메모리 시스템

**저장 위치:** `{{AGENT_MEMORY_DIR}}`

**메모리 유형:**

### user (사용자 프로필)
저장 대상: 사용자 역할, 선호도, 기술 수준, 반복 패턴

### feedback (피드백)
저장 대상: "이렇게 하지 마", "이 방식이 좋았다" 등 명시적·묵시적 피드백
구조:
```markdown
규칙: ...
Why: 사용자가 준 이유
How to apply: 적용 시점과 범위
```

### project (프로젝트 맥락)
저장 대상: 현재 작업 상태, 진행 중인 이슈, 결정 사항
주의: 빠르게 변하므로 자주 업데이트, 절대 날짜로 저장

### reference (외부 참조)
저장 대상: 자주 참조하는 외부 시스템 위치, 문서 경로

---

## 2. 저장하지 않는 정보

```
❌ API key, token, password, credential
❌ PII (이름, 전화, 이메일, 주민번호 등)
❌ 현재 세션에만 유효한 임시 상태
❌ 코드 패턴, 아키텍처 (→ 코드 직접 읽기)
❌ git 히스토리 내용 (→ git log로 확인)
❌ 이미 전역 진입 파일(CLAUDE.md/GEMINI.md/AGENTS.md)에 문서화된 내용
❌ 인수인계 세부 내용 (→ work diary에 기록)
```

---

## 3. Work Diary 운영 (모든 AI 공통)

| AI | Work Diary 경로 |
|----|----------------|
| Claude | `03_ai_notes/work-logs/claude/claude-work-diary.md` |
| Gemini | `03_ai_notes/work-logs/gemini/gemini-work-diary.md` |
| Codex | `03_ai_notes/work-logs/codex/codex-work-diary.md` |

**기록 조건:**
- 의미 있는 작업(파일 생성·수정, 코드 작성, 리서치)을 완료한 후
- 활성 작업 중 {{CHECKPOINT_TIMES}} 시각을 지날 때 (checkpoint)
- AI가 작업하지 않은 시간대는 기록하지 않는다

> **범위 명확화 (time-based checkpoint vs no-time-based-trigger):** 위 {{CHECKPOINT_TIMES}} 시각의 work diary 체크포인트는 **활성 작업 중일 때 명시적으로 허용된다**. {{SHARED_HUB}} 및 {{BOOT_PROTOCOL}}의 "no time-based triggers" 규칙은 자율적 self-scheduling / cron 기반 self-wakeup(에이전트가 스스로 깨어나 작업을 시작하는 것)을 금지하는 것이며, **이 work diary 체크포인트를 금지하는 것이 아니다.** 유휴 시간대에는 어떤 항목도 기록하지 않는다.

**기록 형식:**
```markdown
## YYYY-MM-DD HH:mm [작업 요약]

- 현재 작업: ...
- 마지막 상태: ...
- 확인/수정 파일: ...
- 검증 결과: ...
- 실패/오류: ...
- 잘된 점: ...
- 다음 재개 지점: ...
```

**규칙:** 새 파일을 만들지 않는다. 기존 work diary 파일에 이어 쓴다.

**크기 제한:**
- checkpoint 1개는 `2,000`자 이하를 목표로 한다.
- 긴 명령 출력, 긴 파일 목록, 대형 diff, 테스트 전체 로그는 work diary에 붙여 넣지 않는다.
- 상세 근거가 필요하면 별도 report/log 파일을 만들거나 기존 프로젝트 문서를 갱신하고, work diary에는 요약과 Vault 상대 경로만 남긴다.
- active work diary 파일은 `60,000`자 또는 `100 KB`를 넘기기 전에 월별/주제별 rollover 또는 요약본 작성을 검토한다.
- 이미 기준을 넘은 legacy work diary는 원본 보존 대상이다. 사용자의 별도 승인 없이 대량 분할, 삭제, 이동하지 않는다.
- **기록 분리 기준:** 사건 로그(무엇을 했는지)는 work diary에, 재사용 가능한 교훈은 rollover 시점에 `{{WORK_LOG_DIR}}/<에이전트>/<에이전트>-diary-lessons.md`(교훈 병합본)로 옮긴다. rollover 절차 정본: {{DIARY_ROLLOVER_DOC}}.

---

## 4. 세션 간 컨텍스트 복원

각 세션 시작 시:

```
1. Work diary 마지막 항목 읽기 → "재개 지점"에서 시작
2. 관련 파일 현재 상태 확인 → Work diary의 "관련 파일" 목록 읽기
3. 에이전트 메모리 확인 → 사용자 피드백, 프로젝트 맥락
4. 의도 원장(자동 갱신되는 사용자 의도 프로필) 확인 → {{INTENT_PROFILE_PATH}}
   (자정 배치 자동 재생성; 훅 자동 주입이 없는 에이전트는 수동 확인)
5. 미완료 항목 확인 → Work diary의 체크리스트
```

---

## 5. 인수인계가 필요한 상황

```
A. 세션 종료 후 다른 AI(또는 같은 AI의 다음 세션)가 작업 이어받기
B. 한 AI가 자신의 역할 범위를 벗어난 작업을 다른 AI에게 위임
C. 여러 AI가 동일 프로젝트를 병렬 작업할 때 충돌 방지
D. 작업이 길어 여러 세션에 걸쳐 진행될 때
```

**경로 우선순위:** 1차 경로는 **핸드오프 릴레이(ORDER/REPORT 파일 교환, `{{RELAY_ROOT}}`)** 다. 아래 §6의 work diary 인수인계 형식은 릴레이 비대상 작업(수동·비정형 인수인계, 타 벤더로의 단발 위임)의 정식 경로다. 릴레이 절차 정본: {{RELAY_PROTOCOL_DOC}}.

---

## 6. 인수인계 문서 형식 (work diary 경로 — 릴레이 비대상)

인수인계 내용은 해당 AI의 work diary에 기록한다.

```markdown
## [날짜 시각] 인수인계 — [작업명]

### 현재 담당 AI
Claude / Gemini / Codex

### 다음 담당 AI (또는 미정)
Claude / Gemini / Codex / 미정

### 작업 배경
[왜 이 작업이 시작됐는지]

### 완료된 항목
- [완료 항목 목록]

### 미완료 항목 (다음 AI가 이어받을 것)
- [ ] [항목 1] — [현재 상태, 이어받을 지점]

### 관련 파일 (Vault 상대 경로)
- [수정/생성된 파일]

### 주요 결정 사항 (바꾸면 안 되는 것)
- [설계 결정, 사용자 승인 사항]

### 알려진 이슈·주의사항
- [다음 AI가 알아야 할 문제점]

### 재개 지점
다음 AI는 [구체적 위치/파일]에서 [구체적 작업]부터 시작한다.
```

---

## 7. AI 간 역할 위임

**위임 가능 조건:**
- 현재 AI의 역할 범위(`agent-role-definitions.md`)를 벗어난 작업
- 더 적합한 AI가 명확히 존재
- 사용자가 특정 AI를 지정한 경우
- 핸드오프 릴레이 또는 외부 AI 교차검증 릴레이로 처리 가능한 작업 (외부 벤더 AI 외에 서브에이전트 로스터(예: chief-director, researcher, builder-1 …)도 위임 대상 — 로스터 정의: {{ROSTER_DOC}})

**위임 불가 조건:**
- 동기적(수 초 내) 결과가 필요한 경우
- 현재 AI가 충분히 처리 가능한 작업

**위임 방법:**
```
1. work diary에 인수인계 문서 작성
2. 사용자에게 "이 작업은 [Gemini/Codex]가 처리하면 더 효율적입니다.
   인수인계 내용을 work diary에 기록했습니다." 안내
3. 해당 AI의 work diary에 재개 지점 명시
```

---

## 8. 동시 작업 충돌 방지

여러 AI가 같은 프로젝트를 작업할 때:

```
규칙 1: 같은 파일은 한 번에 한 AI만 수정
규칙 2: 작업 시작 시 work diary에 "현재 [파일명] 수정 중" 기록
규칙 3: 완료 후 work diary에서 잠금 해제 표시
규칙 4: 다른 AI가 수정 중인 파일은 읽기만 하고 수정 안 함
```

충돌 감지 시:
```
1. 수정 멈춤
2. 두 버전 차이 사용자에게 보고
3. 사용자가 어느 버전 유지할지 결정
4. 선택된 버전으로 통합
```

---

## 9. Obsidian Vault 문서 (장기 기억)

AI가 생성한 공식 문서는 Vault에 저장된다. 세션과 무관하게 영구 보존되는 장기 기억이다.

검색 우선 위치:
```
1. {{VAULT_ROOT}}\01_projects\   (활성 프로젝트)
2. {{VAULT_ROOT}}\03_ai_notes\   (AI 기록)
3. {{VAULT_ROOT}}\90_references\ (참조)
4. {{VAULT_ROOT}}\00_inbox\      (미분류)
```

**규칙:** 작업 시작 전 항상 Vault를 검색하고, 관련 기존 문서를 읽는다.

---

*본 문서는 메모리 규칙 + 인수인계 프로토콜 통합본이다. 보관 문서는 `{{VAULT_ROOT}}\99_archive\` 에 배너와 함께 둔다.*
