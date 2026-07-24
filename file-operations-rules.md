---
category: "AI-Governance"
major_category: "AI-Governance"
middle_category: "Harness"
minor_category: "Spec"
category_path: "AI-Governance/Harness/Spec"
tags:
  - "human/korean"
  - "human/warning"
  - "human/decision"
  - "harness"
  - "sync"
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
  - "00_Harness/agent-security-policy.md"
  - "{{SHARED_HUB}}"
  - "{{BOOT_PROTOCOL}}"
---
# <span style="color:#d97706">파일 운영 규칙 (File Operations Rules)</span>
# Harness Engineering v1

Tags: #human/korean #human/warning #human/decision #harness #sync
관련 문서: [[00_Harness/harness-engineering-index.md]] · [[00_Harness/harness-change-history.md]] · [[00_Harness/agent-security-policy.md]] · {{SHARED_HUB}} · {{BOOT_PROTOCOL}}

Vault({{VAULT_ROOT}}) 및 로컬 작업 드라이브 내 모든 파일 작업의 표준을 정의한다.




## 1. Markdown 파일명 규칙

**표준 형식:** `descriptive-title.md`

**예외 형식:** `descriptive-title.excalidraw.md`

| 규칙 | 내용 | 예시 |
|------|------|------|
| 파일명 | 문서 내용을 설명하는 소문자 kebab-case | `ai-master-constitution.md` |
| 날짜 | 파일명이 아니라 `created_at`, `updated_at` frontmatter에 기록 | `created_at: "YYYY-MM-DD"` |
| project | 파일명이 아니라 `project` frontmatter에 기록 | `project: "harness"` |
| type | 파일명이 아니라 `type` frontmatter에 기록 | `type: "spec"` |
| 구분자 | 단어 사이는 하이픈(`-`)만 사용하고 이중 언더스코어(`__`)는 금지 | `task-execution-protocol.md` |

**허용 확장자:**
- `.md`: 일반 문서, 로그, 명세
- `.excalidraw.md`: 다이어그램, 아키텍처, 시각적 요약

**고정 이름 예외:**
- AI 도구가 파일명을 고정해서 읽는 루트 설정 파일: `AGENTS.md`, `GEMINI.md`, `CLAUDE.md` 등 (사용하는 도구의 규약에 따름)
- Obsidian 또는 플러그인이 고정 경로를 요구하는 템플릿·설정 파일
- Daily Note: `03_ai_notes/daily-views/YYYY-MM-DD.md`

**금지:**
- 날짜 prefix가 들어간 신규 Markdown 파일
- project·type을 파일명 prefix로 넣는 방식
- 이중 언더스코어(`__`)를 사용하는 파일명
- 스페이스, 한국어, 대문자, 괄호, 특수문자
- 같은 의미의 짧은 파일명과 표준 파일명을 동시에 유지하는 중복 문서

---

## 2. PDF 파일명 규칙

형식: `descriptive-title.pdf`  
Markdown 파일명 규칙과 동일. 최종 보고서·공유 문서에 사용.

---

## 3. 저장 위치 규칙

```
{{VAULT_ROOT}}\
├── 00_inbox\             임시 보관, 분류 전 항목
├── 00_Harness\           이 하네스 시스템 (AI 운영 지침, 대문자 예외)
├── 00_imported_from_d\   Vault 밖 로컬 드라이브에서 가져온 원본 구조 사본
├── 01_projects\          프로젝트별 작업 문서
│   └── <project-name>\   예: project-a, project-b
├── 02_documents\         프로젝트 무관 일반 문서
├── 03_ai_notes\          AI 작업 기록
│   └── work-logs\
│       ├── claude\
│       ├── gemini\
│       └── codex\
├── 04_attachments\       PDF, 이미지
│   └── <project-name>\
├── {{CONFIDENTIAL_DIR}}\ 기밀 자료 (명명 예외 허용, visibility: pii 기본)
├── 90_references\        외부 참조, 리서치 자료
│   └── <topic>\
└── 99_archive\           완료·비활성 문서
```

**코드·실험 파일:** Vault가 아닌 {{CODE_ROOT}} 또는 지정 작업 폴더에 저장.  
**최종 산출물 Markdown/PDF만** Vault에 저장.

**Standalone HTML 산출물 (정본):** 사용자가 요청한 시각화·보고서·가이드 standalone HTML 문서는 {{HTML_DOCS_DIR}} 한 곳에 생성하고 색인 빌더 스크립트({{HTML_INDEX_BUILDER}})를 실행한다. 프로젝트 종속 HTML(앱 화면·라이브 대시보드·빌드 출력·프로젝트 `docs/`)은 프로젝트 폴더에 유지하고 수집 폴더 색인에 링크만 등록한다. 문서 HTML의 디자인은 {{SHARED_HUB}}가 지정하는 문서 디자인 킷 라우터를 따르고, 일반 "시각화" 요청의 기본 매체는 공개 발행 사이트({{PUBLISH_SITE}}, 예: Quartz 같은 정적 사이트)의 노트다. 클라우드 열람 허브 {{CLOUD_VIEWER}}는 PPT/PPTX·Slides·PDF·`.excalidraw.md`의 **열람·보관용 업로드 허브**로 역할을 한정한다 — standalone HTML을 여기 생성하지 않는다. 프로젝트 종속 원본은 프로젝트 폴더 밖으로 이동하지 않는다. 소속이 불명확하면 `[확인필요]`로 분류한다.

---

## 4. YAML Frontmatter 필수 필드

모든 신규 Markdown 파일에 포함:

```yaml
---
created_at: "YYYY-MM-DD"
updated_at: "YYYY-MM-DD"
project: "project-name"
type: "spec"
tool: "claude"
status: "draft"
visibility: "internal"
source_path: ""
related:
  - ""
---
```

**type 허용값:** `readme` `plan` `spec` `log` `research` `troubleshoot` `meeting` `summary` `index` `archive` `handoff` `guide` `report` `checklist` `template` `note`  
**tool 허용값:** `claude` `gemini` `chatgpt` `codex` `manual` `auto-generated`  
**status 허용값:** `draft` `active` `done` `archived`  
**visibility 허용값:** `public` `internal` `pii` — 공개 발행 사이트({{PUBLISH_SITE}}) 발행 게이트. **미지정(기본값)은 `internal`로 간주한다.**

| 값 | 의미 | 발행 |
|------|------|------|
| `public` | 공개 사이트로 발행 대상 | `draft: false`와 함께여야 발행. 큐레이션 리라이트를 거쳐 나간다 |
| `internal` | Vault 내부 전용 (기본값) | 절대 공개되지 않는다 |
| `pii` | 개인정보·기밀 | 발행 절대 금지. {{CONFIDENTIAL_DIR}} 기밀 파일 등 |

> **게이트는 소스에서 강제한다.** 발행 시점 수동 판정이 아니라 이 필드가 진실의 원천이다. 발행 대장은 {{PUBLISH_SITE}} 발행 맵 문서에 둔다.
> **성격상 기계 생성물·데이터·로그(예: 자동 생성 도구 폴더, `03_ai_notes/work-logs`, `99_archive`)는 항상 `internal`이며 공개 승격 대상이 아니다.**
> 기존 노트는 미지정 상태이므로 안전측 기본값 `internal`이 적용된다. 공개할 노트에만 `visibility: public`을 명시적으로 부여한다(백필은 점진적).

> **정본 각주:** 이 §4의 type 16종·tool 6종·status 4종·visibility 3종이 하네스 전체의 정본이다. {{BOOT_PROTOCOL}}의 구 enum과 상충하면 **본 문서 §4가 이긴다.**

---

## 5. AI Context Size Budget

AI가 읽고 요약하고 검증할 수 있도록 Markdown과 코드 파일은 아래 크기 기준을 따른다.

| 대상 | 목표 기준 | 분할/조치 기준 |
|------|-----------|----------------|
| 일반 Markdown | `12,000`자 이하 또는 `20 KB` 이하 | `25,000`자 또는 `40 KB` 초과 시 index + child notes로 분리 |
| 부팅/설정/인덱스/인수인계 문서 | `8,000`자 이하 또는 `15 KB` 이하 | 초과 시 핵심 규칙만 남기고 세부 내용은 별도 spec/log로 분리 |
| work diary 항목 1개 | `2,000`자 이하 | 상세 근거는 별도 report/log에 쓰고 diary에는 링크와 요약만 기록 |
| active work diary 파일 | `60,000`자 이하 또는 `100 KB` 이하 | 초과 전 월별/주제별 rollover 또는 요약본 작성 |
| Markdown 표 | `100`행 이하 | 초과 시 CSV, JSONL, SQLite, 또는 프로젝트-local data 파일 사용 |
| Markdown 내부 코드블록 | `120`줄 이하 | 초과 시 Vault 밖 코드 파일로 저장하고 문서에는 경로와 요약만 기록 |
| AI 작성/대규모 수정 코드 파일 | `600`줄 이하 또는 `40 KB` 이하 | `800`줄/`60 KB` 초과 시 모듈 분리 검토, `1,000`줄/`80 KB` 초과 시 분리 계획 필수 |

> **RAG 색인 하드 상한:** {{RAG_DIR}}의 ingest 스크립트는 `120,000` bytes를 초과하는 파일을 오류 없이 "스킵(large-file)"으로 조용히 건너뛰고 색인에서 제외한다. 위 표의 목표치와 별개로 이 값은 분할의 하드 상한으로 간주한다 — 넘기면 RAG 검색에서 그 문서가 존재하지 않는 것처럼 보인다.

운영 규칙:

1. 새 Markdown이 목표 기준을 넘을 것으로 보이면 처음부터 index note와 child note로 나눈다.
2. 기존 대형 파일을 읽을 때는 전체를 한 번에 읽지 말고 headings, symbol search, `rg`, line window로 필요한 부분만 읽는다.
3. 기존 대형 Markdown을 임의로 분할, 이동, 삭제하지 않는다. 먼저 분할 정리안을 제시한다.
4. `.excalidraw.md`는 drawing JSON 때문에 예외로 허용한다. 단, 중요한 다이어그램은 `4,000`자 이하의 요약 Markdown 또는 본문 요약 섹션을 함께 둔다.
5. source corpus, 대량 로그, RAG chunk, 테스트 fixture는 단일 Markdown에 누적하지 않는다. CSV/JSONL/SQLite 또는 Vault 밖 프로젝트 data 파일을 사용한다.

---

## 6. 파일 수정 규칙

| 작업 | 규칙 |
|------|------|
| 내용 수정 | `updated_at` 날짜 업데이트 |
| 이름 변경 | 백업 먼저, 전/후 경로 보고, Obsidian 링크 확인 |
| 이동 | 목적지 폴더가 규칙에 맞는지 확인 후 이동 |
| 삭제 | 사용자 확인 필수, 되돌릴 수 없음 명시 |
| 대량 작업 | 50개 이상은 에스컬레이션 |
| 크기 초과 | 위 AI Context Size Budget을 확인하고 분할/요약/정리안을 우선 적용 |

---

## 7. Obsidian 내부 링크 규칙

```
✅ 링크: 근거 관계가 명확한 경우 (A 문서가 B 문서를 직접 참조)
❌ 링크: 같은 폴더에 있다는 이유만
❌ 링크: 비슷한 주제라는 이유만
❌ 링크: 링크 수를 늘리기 위한 목적
```

링크 형식: `[[vault-relative-path/filename]]` 또는 `[[filename]]`  
파일이 존재하는지 확인 후 링크 추가.

---

## 8. 폴더 명명 규칙

```
✅ 소문자, 숫자, 하이픈(-), 언더스코어(_)
❌ 스페이스, 한국어(또는 현지어), 대문자 (단, `00_Harness`, {{CONFIDENTIAL_DIR}} 등 명시된 예외 제외)
```

루트 폴더 고정 구조:
`00_inbox` `00_Harness` `00_imported_from_d` `01_projects` `02_documents` `03_ai_notes` `04_attachments` `{{CONFIDENTIAL_DIR}}` `90_references` `99_archive`

---

## 9. 클라우드 동기화 및 백업 정책

클라우드 드라이브 루트 구조:

```text
<클라우드 드라이브>/
├── {{REMOTE_SYNC_DIR}}/    Obsidian 원격 동기화 대상
├── {{CLOUD_VIEWER}}/       HTML/PPT/Slides/PDF/Excalidraw_MD 열람 허브
└── 99_archive/             기존 클라우드 항목 보존 아카이브
```

운영 원칙:

- {{VAULT_ROOT}} 로컬 Vault를 기준 원본(canonical source)으로 본다.
- {{REMOTE_SYNC_DIR}}는 동기화 대상이지 임시 작업 폴더가 아니다.
- 클라우드 열람 허브 {{CLOUD_VIEWER}}는 PPT/PPTX·Slides·PDF·`.excalidraw.md`의 **열람·보관용 업로드 허브**다. 프로젝트-bound 원본 저장소가 아니며, standalone HTML은 여기 두지 않는다(§3 정본 — {{HTML_DOCS_DIR}}).
- 사용자 요청으로 생성되는 단독 PPT/PPTX/Slides/PDF/`.excalidraw.md`는 {{CLOUD_VIEWER}} 하위 `PPT`, `Slides`, `PDF_Exports`, `Excalidraw_MD` 중 맞는 폴더에 둔다.
- `.excalidraw.md`는 Obsidian 편집 가능한 원본 성격이 강하므로 프로젝트 종속 원본은 로컬/Vault에 유지하고, {{CLOUD_VIEWER}}의 `Excalidraw_MD`에는 보관 copy를 둔다.
- AI는 사용자 승인 없이 클라우드 드라이브 업로드, 삭제, 이동, 권한 변경, 공유 링크 생성을 하지 않는다.
- 단, 사용자가 승인한 이 정책 범위 안에서 새 단독 열람 산출물(PPT/Slides/PDF/excalidraw)을 {{CLOUD_VIEWER}}에 생성·업로드·연결하는 것은 허용된다. 삭제, 권한 변경, 공유 설정 변경은 여전히 별도 승인이 필요하다.
- 클라우드의 `99_archive`는 과거 파일 보존용이며, 로컬 Vault의 `99_archive`와 자동 병합하지 않는다.
- **예외**: 발행 전면 금지 경로 목록 {{SEALED_PATHS}} 하위 및 그 파생물은 어떤 외부 업로드·링크·변환 대상도 아니다(봉인 저장소).

동기화 트리거:

- 기본 원본은 로컬 {{VAULT_ROOT}}이며, 클라우드의 {{REMOTE_SYNC_DIR}}는 Markdown 미러다.
- 사용자가 동기화 자동화를 승인한 세션에서는 동기화 워처 스크립트({{SYNC_WATCHER_SCRIPT}})가 Markdown 변경을 감지해 클라우드 미러로 복사할 수 있다.
- 구조 변경, 대량 이름 변경, 충돌 정리 전에는 timestamp 백업을 먼저 만든다.
- 삭제·권한 변경·공유 링크 생성·비Markdown 업로드는 자동 watcher가 수행하지 않는다.
- destructive 옵션 또는 mirror delete 옵션을 도입할 때는 먼저 dry-run으로 변경 예정 경로, 파일 수, 충돌 후보를 출력한다.

백업 주기:

- 이름 변경, 대량 수정, 동기화 전에는 {{BACKUP_ROOT}} 아래에 timestamp 백업을 만든다.
- 백업 폴더명은 `작업명-YYYYMMDD-HHMMSS` 형식을 사용한다.
- `.obsidian\plugins\<plugin>\data.json`처럼 credential이 포함될 수 있는 파일은 일반 백업·동기화 대상에서 제외하고, 암호화 백업 또는 재발급으로 처리한다.

충돌 해결:

- 로컬과 클라우드 양쪽에 다른 버전이 있으면 덮어쓰지 않는다.
- 충돌 파일은 원본 둘 다 보존하고 `_conflict_YYYYMMDD-HHMMSS` 접미사를 붙여 별도 보관한다.
- 어느 버전을 기준으로 통합할지 불명확하면 사용자에게 확인한다.
- 클라우드 쪽 파일이 더 최신이어도 로컬 Vault 규칙과 frontmatter 검증을 통과하기 전에는 기준 문서로 승격하지 않는다.

---

## 10. Vault 외부 로컬 파일 가져오기

Vault 밖 로컬 드라이브의 파일을 Vault로 가져올 때:
- 원본을 이동하지 않는다
- `{{VAULT_ROOT}}\00_imported_from_d\` 아래에 원본 디렉터리 구조를 유지하며 복사
- 예: `{{CODE_ROOT}}\project-a\spec.md` → `{{VAULT_ROOT}}\00_imported_from_d\<원본 상위 폴더>\project-a\spec.md`

---

## 11. Daily Note 자동 생성 규칙

Calendar 플러그인 연동을 위해 AI는 파일 생성 작업이 있는 날 Daily Note를 생성해야 한다.

**Daily Note 경로:** `03_ai_notes/daily-views/YYYY-MM-DD.md`  
**템플릿:** `03_ai_notes/daily-views/_template-daily.md`

| 상황 | 행동 |
|------|------|
| 세션 중 새 파일 생성 시 | 당일 Daily Note 존재 여부 확인 |
| Daily Note 없음 | 템플릿 복사 후 `{{date:YYYY-MM-DD}}` → 실제 날짜로 치환하여 생성 |
| Daily Note 이미 존재 | 아무것도 하지 않음 (Dataview가 자동으로 파일 목록 갱신) |
| 세션 완료 보고 전 | Daily Note가 생성되었는지 확인 |

**주의:** Daily Note 자체는 Dataview 쿼리 컨테이너이다. 내용을 직접 편집하거나 파일 목록을 수동으로 추가하지 않는다. 쿼리는 `created_at` frontmatter 기준으로 자동 집계된다.

---

*최초 작성: YYYY-MM-DD*
