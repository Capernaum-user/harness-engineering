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
  - "00_Harness/ai-master-constitution.md"
  - "00_Harness/task-execution-protocol.md"
  - "00_Harness/agent-decision-rules.md"
---
# 완료 기준 및 품질 관리 (Task Completion Standards)
# Harness Engineering v1

Tags: #harness #quality #acceptance
관련 문서: [[00_Harness/harness-engineering-index.md]] · [[00_Harness/ai-master-constitution.md]] · [[00_Harness/task-execution-protocol.md]] · [[00_Harness/agent-decision-rules.md]]

"완료"의 정의, 품질 기준, 단계별 체크리스트를 하나로 통합한 참조 문서.
완료 보고 전 반드시 통과해야 한다.

**이 문서가 완료 기준(완료·검수·품질)의 유일 정본이다.** 이 문서로 대체된 구 문서(개별 완료 기준·검수 체크리스트·품질 관리 문서)는 {{VAULT_ROOT}}\99_archive\ 에 배너와 함께 보관한다.

## 1. 공통 완료 기준 (모든 작업)

- [ ] 요청 범위 내의 모든 항목을 처리했다
- [ ] 처리하지 못한 항목이 있으면 이유와 함께 명시했다
- [ ] 작업 중 발생한 오류가 있으면 기록했다
- [ ] 의미 있는 작업이면 해당 AI work diary에 기록했다

---

## 2. 세션 시작 체크리스트

```
[ ] 0. ai-master-constitution.md + {{BOOT_PROTOCOL}} (부팅 필수 2문서)
[ ] 1. Shared Hub 확인 ({{SHARED_HUB}})
[ ] 2. 하네스 관련 파일 필요 시 참조 (00_Harness/, {{BOOT_PROTOCOL}} 조건부 로드 표 기준)
[ ] 3. {{KNOWLEDGE_MAP_INDEX}}가 있으면 먼저 확인
[ ] 4. 현재 작업과 관련된 Vault 문서 검색 (Knowledge Map 후 RAG)
[ ] 5. Work diary에서 이전 세션 재개 지점 확인
[ ] 6. 에이전트 메모리에서 피드백·맥락 확인 (지원되는 도구 전용)
[ ] 7. 작업 범위와 저장 위치 결정
```

---

## 3. 신규 Vault 문서 체크리스트

```
파일명
[ ] 형식: descriptive-title.md
[ ] 소문자 kebab-case, 특수문자·한국어 없음, **2~5 단어**
[ ] type: 정본 [[00_Harness/file-operations-rules.md]] §4의 16종 중 하나

Frontmatter
[ ] created_at: YYYY-MM-DD
[ ] updated_at: YYYY-MM-DD
[ ] project: 실제 프로젝트명
[ ] type: 정본 §4의 16종 중 하나
[ ] tool: claude / gemini / chatgpt / codex / manual / auto-generated
[ ] status: draft / active / done / archived
[ ] visibility: public / internal / pii (기본 internal, 기밀 문서({{CONFIDENTIAL_DIR}})는 pii 명시, 발행 대상만 public)
[ ] source_path: (있으면 작성)
[ ] related: (근거 관계 있는 경우만)

저장 위치 및 내용
[ ] 01_projects/<project-name>/ 또는 적절한 위치
[ ] API key, token, PII 없음
[ ] 내부 링크는 근거 관계 명확한 경우만
[ ] 한국어 기본 (사람이 읽는 문서)
[ ] 기술 용어는 영문 원문 + 한국어 설명 혼용 가능 (코드·경로·API 이름은 항상 영어)
[ ] 일반 Markdown은 12,000자/20 KB 목표 준수
[ ] 25,000자 또는 40 KB 초과 예상 시 index + child notes로 분리
[ ] Markdown 표 100행 초과 시 CSV/JSONL/SQLite 사용
[ ] Markdown 추가/대규모 수정 후 지식맵 생성 스크립트({{RAG_DIR}}\generate_index_map.py) 실행
```

---

## 4. 코드 작성/수정 체크리스트

### 품질 기준

```
✅ 실행 가능 (문법 오류 없음)
✅ 요청한 기능 구현됨
✅ 타입 힌트 (Python)
✅ 예외 처리 (try/except 또는 Result)
✅ 보안: SQL injection, path traversal, 하드코딩 시크릿 없음
✅ 단일 책임 원칙 (함수·클래스)

❌ 축약·생략·"..." 사용 (완성본이 아닌 코드)
❌ 하드코딩된 경로, IP, 포트 (환경 변수나 설정 파일로 분리)
❌ print() 디버그 코드 남기기 (배포 코드)
```

### 권장 (복잡한 코드)

```
✅ 시간/공간 복잡도 주석 (O-notation, 임계 구간)
✅ docstring (비명시적 public API)
✅ 로그 또는 출력으로 실행 상태 확인 가능
✅ requirements.txt 또는 의존성 명시
```

### 체크리스트

```
기본
[ ] 실행 가능한 완성본 (축약·생략·"..." 없음)
[ ] 타입 힌트 (Python)
[ ] 예외 처리 (try/except 또는 결과 타입)
[ ] 요청 범위만 수정 (불필요한 추가 리팩토링 없음)

보안
[ ] SQL injection 없음
[ ] path traversal 없음
[ ] 하드코딩된 시크릿 없음 → 환경 변수 사용
[ ] 불필요한 권한 요청 없음

품질
[ ] 단일 책임 원칙 (함수·클래스)
[ ] 의존성과 버전 명시
[ ] print() 디버그 코드 제거 (배포 코드)
[ ] .gitignore에 .env, *.key 등 포함
[ ] AI 작성/대규모 수정 코드 파일은 600줄/40 KB 목표 준수
[ ] 800줄/60 KB 초과 시 모듈 분리 검토
[ ] 1,000줄/80 KB 초과 시 분리 계획 작성

수정 시 추가
[ ] 기존 동작 유지 확인
[ ] 변경 범위와 이유 설명 준비
[ ] 인터페이스 변경 여부 명시
[ ] 기존 테스트가 통과하거나, 테스트가 없으면 수동 검증 방법 제시
[ ] 버그 수정이면: 같은 패턴의 다른 버그가 있는지 확인하고, 있으면 별도 보고
```

---

## 5. 리서치·보고서 체크리스트

```
[ ] Vault에 관련 기존 문서 없음 (중복 방지 검색 완료)
[ ] 정보 출처(날짜, URL 또는 문서명) 명시
[ ] 불확실한 정보 "확인 필요" 표시
[ ] 핵심 인사이트 TL;DR로 요약
[ ] Vault 관련 문서와 비교·연결
[ ] 기밀 프로젝트({{CONFIDENTIAL_DIR}}) 관련이면 기밀 분류 확인
[ ] 5개 초과 파일 일괄 작업(rename/move/delete)·인덱스 재생성 전 git checkpoint 커밋 완료 (전역 진입 파일(CLAUDE.md/GEMINI.md/AGENTS.md) 규칙)
```

---

## 6. 자동화·스크립트 체크리스트

```
[ ] 실행 환경 명시 (OS, Python 버전 등)
[ ] 예외 상황 처리 (파일 없음, 권한 없음, 네트워크 오류)
[ ] 로그/출력으로 실행 상태 확인 가능
[ ] 파괴적 동작 있으면 dry-run 또는 확인 프롬프트
[ ] 스케줄 작업이면 실패 시 알림 방법 있음
[ ] 환경 변수로 설정 관리 (하드코딩 없음)
[ ] Vault Markdown을 생성/수정하는 스크립트면 지식맵 생성 스크립트 호출 지점 명시
```

---

## 7. 완료 보고 체크리스트

```
[ ] 처리된 항목 목록 작성
[ ] 미처리 항목 + 이유 명시
[ ] 발생한 오류/주의사항 기록
[ ] 검증 결과 명시
[ ] 참고한 Vault 문서 경로 나열
[ ] 의미 있는 작업이면 work diary 업데이트
[ ] Markdown 추가/수정 시 RAG 재색인 실행: `& "{{RAG_PYTHON}}" {{RAG_DIR}}\ingest.py` — 출력의 "스킵(large-file)" 경고를 확인하고, 걸린 파일은 분할 검토
[ ] Markdown 추가/수정 작업이면 `python {{RAG_DIR}}\generate_index_map.py` 실행 결과 확인 (RAG DB 갱신 = ingest.py, 지식맵 갱신 = generate_index_map.py — 문서 작업 후 둘 다)
[ ] 인수인계 필요하면 session-continuity-protocol.md 형식으로 기록
[ ] 핸드오프 릴레이(ORDER/REPORT 파일 교환) 작업이면 이 절의 보고 양식 대신 **REPORT 형식(order_id 에코 필수)**으로 완료 보고하며, 완료 수락 기준은 릴레이 검토 절차의 **3중 게이트**(echo: order_id 일치 / evidence: 수용 기준 커버리지 / boundary: allowed_paths 준수) 통과다. 규격: {{SHARED_HUB}}의 핸드오프 릴레이 프로토콜 문서.
```

---

## 8. 에스컬레이션 판단 체크리스트

다음 중 하나라도 해당하면 사용자에게 확인 후 진행:

```
[ ] 파일 삭제 (1개 이상)
[ ] 50개 이상 파일 동시 수정
[ ] git push, 원격 배포
[ ] 외부 서비스로 데이터 전송
[ ] 의료·법률·금융 판단 확정
[ ] 인증 정보, 권한 변경
[ ] 요청 해석이 두 방향 이상이고 결과가 크게 다름
[ ] 기밀 문서({{CONFIDENTIAL_DIR}}) 정보가 포함된 외부 전송
```

---

## 9. AI 자체 검토 절차

자체 검토는 **이관 전 최소 위생 점검**이다. 정식 QA는 구현 세션과 분리된 격리 테스터 세션이 수행한다 (삼권분립, {{SHARED_HUB}}의 "AI Collaboration QA Standard").

작업 완료 후, 완료 보고 전:

```
Step 1: 요청과 산출물 비교
  - 요청에서 지시한 모든 항목이 반영됐는가?
  - 요청하지 않은 항목을 임의로 추가하지 않았는가?

Step 2: 해당 체크리스트 통과 (위 3~8 중 해당 항목)
  - 통과 못한 항목은 완료 보고에 명시

Step 3: 보안 스캔
  - 생성한 파일에 API key, token, PII 없음
  - 외부 업로드 없음

Step 4: 파일 검증
  - 생성·수정한 파일이 실제로 존재하는가?
  - 내용이 예상대로 저장됐는가?
```

---

## 10. 코드 리뷰 기준

핸드오프 릴레이 작업의 검토는 릴레이 검토 절차의 3중 게이트를 따른다. 이 절의 CRITICAL~LOW 형식은 릴레이 외 일반 코드 리뷰에 적용한다.

다른 AI가 작성한 코드를 검토할 때:

| 등급 | 설명 | 조치 |
|------|------|------|
| CRITICAL | 보안 취약점, 데이터 손실 가능 | 즉시 수정 필요, 사용자 알림 |
| HIGH | 기능 오류, 실행 불가 | 수정 필요 |
| MEDIUM | 성능, 유지보수성 문제 | 개선 권고 |
| LOW | 스타일, 관행 차이 | 선택적 개선 |

```markdown
## 코드 리뷰 결과

### CRITICAL
- [ ] ...

### HIGH
- [ ] ...

### 통과한 항목
- ...

### 종합 판단: 승인 / 수정 필요
```

---

## 11. 완료 보고 형식

```
## 완료 보고

### 처리된 항목
- ...

### 미처리 항목 (이유 포함)
- ...

### 발생한 오류/주의사항
- ...

### 검증 결과
- ...

### 참고한 Vault 문서
- ...
```

---

## 12. 문서 리뷰 기준

완성된 Vault 문서를 검토할 때 (다른 AI가 작성한 문서 포함):

```
파일명 규칙 ✅/❌
frontmatter 완성 ✅/❌
저장 위치 적합 ✅/❌
내용 정확성 ✅/❌ (확인 가능한 경우)
보안 (PII, 시크릿 없음) ✅/❌
링크 유효성 ✅/❌
```

---

## 13. 회귀 방지

수정 작업 후 기존 기능이 깨지지 않는지 확인:

```
1. 수정된 파일의 주변 코드 확인
2. 수정이 영향을 주는 다른 파일 확인
3. 인터페이스(함수 시그니처, API 엔드포인트) 변경 여부 보고
4. 기존 테스트가 있으면 통과 여부 확인
```

---

## 14. AI 협업·인수인계 작업 체크리스트

```
[ ] 인수인계 형식은 [[00_Harness/session-continuity-protocol.md]] §6을 따름
[ ] 다음 AI가 이어받을 수 있는 재개 지점이 명확히 표시됨
[ ] 미완료 항목이 번호 목록으로 나열됨
```
