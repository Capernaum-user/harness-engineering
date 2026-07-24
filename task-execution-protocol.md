---
category: "AI-Governance"
major_category: "AI-Governance"
middle_category: "Harness"
minor_category: "Protocol"
category_path: "AI-Governance/Harness/Protocol"
tags:
  - "human/korean"
  - "human/decision"
  - "harness"
  - "execution"
  - "category/ai-governance"
  - "category/ai-governance/harness"
  - "category/ai-governance/harness/protocol"
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
  - "{{SHARED_HUB}}"
  - "{{BOOT_PROTOCOL}}"
  - "00_Harness/agent-decision-rules.md"
  - "00_Harness/task-completion-standards.md"
---
# 작업 실행 프로토콜 (Task Execution Protocol)
# Harness Engineering v1

Tags: #human/korean #human/decision #harness #execution
관련 문서: [[00_Harness/harness-engineering-index.md]] · [[00_Harness/harness-change-history.md]] · {{SHARED_HUB}} · {{BOOT_PROTOCOL}} · [[00_Harness/agent-decision-rules.md]]

AI가 작업을 시작부터 완료까지 처리하는 표준 절차.

## 세션 부팅 순서

```
1. 현재 사용자 요청과 활성 시스템 지시 확인
2. 전역 진입 파일 로드 (CLAUDE.md/GEMINI.md/AGENTS.md 등 — 자동)
3. {{SHARED_HUB}} 확인
4. 의도 원장(자동 갱신되는 사용자 의도 프로필) 확인 — `{{INTENT_LEDGER_PATH}}`
   (훅 자동 주입이 설정된 에이전트는 자동, 그 외 에이전트는 수동 확인)
5. 기획/구현/테스트 중 자신의 세션 역할 식별 (삼권분립 — agent-role-definitions.md)
6. 이 하네스(00_Harness/) 관련 파일 참조
7. Vault에서 작업 관련 기존 문서 검색
8. 관련도 높은 문서 읽기
9. Work diary에서 이전 세션 재개 지점 확인
10. 작업 범위와 저장 위치 결정
```

---

## 작업 실행 흐름

### 단계 1: 요청 분석

```
- 무엇을 만들거나 수정하거나 조사하는가?
- 요청 범위가 명확한가? 불명확하면 가정 명시 또는 확인
- 에스컬레이션 조건(agent-decision-rules.md)에 해당하는가?
- 어느 AI의 주 역할 영역인가? (agent-role-definitions.md)
```

### 단계 2: Vault 사전 검색 (RAG 우선)

Vault에는 **의미 검색(RAG) 인덱스**가 구축되어 있다. 파일명 키워드 검색보다 먼저 RAG 쿼리를 사용한다.

```bash
# 동작이 확인된 전용 인터프리터. query.py는 의존성이 없으면 이 경로로 자동 재실행(re-exec)한다.
RAG_PY="{{RAG_PYTHON}}"

# 기본 검색 (top 5)
"$RAG_PY" {{RAG_DIR}}\query.py "검색 키워드"

# 더 많은 결과 / 필터 / JSON 출력
"$RAG_PY" {{RAG_DIR}}\query.py "검색어" --top 8 --project <project-name> --type spec
"$RAG_PY" {{RAG_DIR}}\query.py "검색어" --json
```

시스템 `python`으로 호출해도 query.py가 위 venv로 자동 재실행하므로 동작은 한다. 다만 명시적 호출을 권장한다.

결과에서 `similarity >= 0.5` 이상인 문서를 우선 읽는다.  
DB 업데이트(새 파일 추가 후): `"$RAG_PY" {{RAG_DIR}}\ingest.py`

> **한계 고지:** `ingest.py`는 `120,000` bytes를 초과하는 파일을 "스킵(large-file)"으로 조용히 건너뛰고 색인에서 제외한다. RAG 검색 결과에 없다고 해서 그 문서가 없거나 낡았다는 뜻이 아닐 수 있다 — Glob/Grep으로 보완 확인한다.

```
Vault 검색 절차:
1. RAG 쿼리로 의미 유사 문서 탐색
2. similarity 높은 문서 Read로 읽기
3. 기존 문서가 있으면 중복 방지
4. 저장 위치 확인 (01_projects/<project> 또는 다른 위치)
5. `40 KB` 초과 파일은 전체 읽기 전에 headings, `rg`, line window로 필요한 범위를 먼저 좁힘
```

### 단계 3: 작업 실행

**문서 작업:**
```
1. 파일명 결정 (descriptive-title.md)
2. 저장 위치 확인
3. frontmatter 작성 (visibility 필드 포함)
4. 내용 작성
5. AI context size budget 확인
   - 일반 Markdown: 12,000자/20 KB 목표
   - 25,000자 또는 40 KB 초과 예상 시 index + child notes로 분리
6. 내부 링크 추가 (근거 관계 명확한 경우만)
```

**코드 작업:**
```
1. 기존 코드 파악 (관련 파일 읽기)
2. 변경 최소 범위 결정
3. 구현 (요청 범위만)
4. 자체 검증 (문법, 타입, 보안)
5. 코드 크기 확인
   - AI 작성/대규모 수정 파일: 600줄/40 KB 목표
   - 800줄/60 KB 초과 시 모듈 분리 검토
   - 1,000줄/80 KB 초과 시 분리 계획 필수
6. 변경 사항 설명 준비
```

**리서치 작업:**
```
1. 기존 Vault 리서치 확인 (중복 방지)
2. 정보 수집 (WebSearch, WebFetch)
3. 출처 기록
4. 인사이트 정리
5. Vault 문서로 저장
```

**5개 초과 파일 일괄 작업(rename/move/delete)·인덱스 재생성 전:** git checkpoint 커밋 필수 (Vault 진입 파일 규칙) — `git add -A && git commit -m "checkpoint: before bulk file op"`. 실패 시 `git reset --hard <checkpoint>`로 즉시 복원.

### 단계 4: 자체 검증 및 RAG DB 자동 갱신

> **적용 범위:** 아래 자체 검증은 **단독 작업 세션에만** 적용한다. 삼권분립 협업(Creator 세션)은 자가 테스트로 완료를 선언하지 않고 이관한다 — 정식 검증은 격리된 Tester 세션이 수행한다.

```
- task-completion-standards.md 해당 체크리스트 통과 확인
- 처리 못한 항목 있으면 이유와 함께 메모
- 보안 검사: API key, PII 없음 확인
- 파일 크기 검사: 신규/수정 Markdown과 AI 작성/대규모 수정 코드가 size budget을 지키는지 확인
- [필수] Vault 내 파일을 하나라도 생성/수정/삭제했다면, 사용자에게 보고하기 전에 AI가 스스로 다음 두 명령을 실행할 것 (역할이 다르다 — **RAG DB = ingest.py, 지식맵 = generate_index_map.py**, 문서 작업 후 둘 다 필요):
  "{{RAG_PYTHON}}" {{RAG_DIR}}\ingest.py
  python {{RAG_DIR}}\generate_index_map.py
```

> **ingest 잠금 오류 대응:** ingest가 잠금 오류로 실패하면 `{{RAG_DIR}}\ingest.lock` 폴더 잔류 여부와 실행 중인 ingest 프로세스 부재를 확인한 뒤 잠금 폴더를 제거하고 재실행한다.

### 단계 5: 완료 보고

```
## 완료 보고

### 처리된 항목
- (처리한 것 목록)

### 미처리 항목
- (못 한 것 + 이유)

### 발생한 오류/주의사항
- (오류, 예외, 알아야 할 것)

### 참고한 Vault 문서
- (Vault 상대 경로)
```

**릴레이 세션 분기:** 핸드오프 릴레이(ORDER/REPORT 파일 교환)를 경유한 릴레이 세션에서는 위 양식 대신 **ORDER 수용 기준 에코 + REPORT 양식(order_id 에코 필수)**이 우선한다. 절차 중복 기술 금지 — 상세는 [[00_Harness/agent-handoff-protocol.md]]와 {{SHARED_HUB}}의 릴레이 프로토콜 문서를 따른다.

### 단계 6: Work Diary 업데이트

의미 있는 작업이면 해당 AI work diary에 기록:
```
{{VAULT_ROOT}}\03_ai_notes\work-logs\<agent>\<work-diary>.md
```

---

## 도구 사용 우선순위 (Claude 기준)

| 목적 | 1순위 | 2순위 | 금지 |
|------|-------|-------|------|
| **Vault 의미 검색** | `python {{RAG_DIR}}\query.py` | Grep | — |
| 파일 읽기 | Read | — | cat (Bash) |
| 파일 쓰기 | Write/Edit | — | echo > (Bash) |
| 파일명 패턴 검색 | Glob, Grep | — | find (Bash) 단독 |
| Vault 조작 | mcp__obsidian__* | Read/Write | — |
| 시스템 정보 | PowerShell | Bash | — |

---

## 병렬 실행 규칙

독립적인 작업(의존성 없음)은 병렬로 실행하여 속도를 높인다:
```
✅ 병렬: 여러 파일 동시 읽기, 독립된 파일 동시 생성
✅ 병렬: 여러 검색 쿼리 동시 실행
❌ 병렬: 이전 결과가 필요한 순서 있는 작업
❌ 병렬: 같은 파일에 동시 쓰기
```

---

## 중단·재시작 규칙

작업이 중단될 때:
1. 현재까지 완료된 항목을 work diary에 기록
2. 재개 지점을 명확히 명시
3. 미완료 항목 목록 작성

재시작 시:
1. Work diary에서 재개 지점 확인
2. 완료된 파일 상태 확인 (중복 방지)
3. 미완료 항목부터 재개
