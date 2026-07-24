# 플레이스홀더 치환표

템플릿 전체에서 `{{...}}` 를 자기 환경 값으로 전역 치환하십시오. 경로 예시는 형식일 뿐이며 OS·드라이브에 맞게 적으면 됩니다.

| 플레이스홀더 | 의미 | 예시 형식 |
|---|---|---|
| `{{VAULT_ROOT}}` | Obsidian Vault 절대경로 | `/path/to/vault` |
| `{{CODE_ROOT}}` | 코드 프로젝트 루트 (Vault 밖) | `/path/to/projects` |
| `{{ARCHIVE_ROOT}}` | 백업·아카이브 루트 (Vault 밖) | `/path/to/archive` |
| `{{HTML_DOCS_DIR}}` | 산출물 HTML 단일 수집 폴더 | `/path/to/html-docs` |
| `{{RELAY_ROOT}}` | 핸드오프 릴레이(ORDER/REPORT) 폴더 | `/path/to/relay` |
| `{{RAG_DIR}}` | 의미 검색(RAG) 스크립트 폴더 | `{{VAULT_ROOT}}/.rag` |
| `{{RAG_PYTHON}}` | RAG 전용 파이썬 인터프리터 | `/path/to/venv/python` |
| `{{CONFIDENTIAL_DIR}}` | 기밀 문서 전용 폴더 (Vault 내) | `{{VAULT_ROOT}}/NN_secret` |
| `{{SEALED_PATHS}}` | 외부 발행 전면 금지 경로 목록 | `/path/one; /path/two` |
| `{{CLOUD_VIEWER}}` | 외부 형식(PPT/PDF 등) 열람용 클라우드 허브 | 클라우드 폴더 링크 |
| `{{PUBLISH_SITE}}` | 공개 발행 사이트(예: Quartz) 루트 | `/path/to/site` |
| `{{SHARED_HUB}}` | 도구 공통 설정 허브 문서 경로 | `{{VAULT_ROOT}}/.../shared-hub.md` |
| `{{BOOT_PROTOCOL}}` | 통합 부팅 프로토콜 문서 경로 | `{{VAULT_ROOT}}/.../boot-protocol.md` |
| `{{WORK_LOG_DIR}}` | AI 작업 일지 폴더 | `{{VAULT_ROOT}}/03_ai_notes/work-logs` |
| `{{USER_NAME}}` | 최종 의사결정권자(사용자) 호칭 | 이름 또는 역할명 |
| `{{ORG_NAME}}` | 조직·회사명 (없으면 삭제) | — |
| `{{CHECKPOINT_TIMES}}` | 작업 중 일지 체크포인트 시각 목록 | `00:00/12:00/18:00` |
| `{{DOCUMENT_LANE_ROUTER}}` | 문서 HTML 디자인 규격 라우터 문서 (없으면 해당 줄 삭제) | — |
| `{{PRESENTATION_KITS_INDEX}}` | 발표물 디자인 킷 인덱스 (없으면 해당 줄 삭제) | — |
| `{{HTML_INDEX_BUILDER}}` | HTML 수집 폴더 색인 재생성 스크립트 | `/path/to/index-builder` |
| `{{INTENT_LEDGER_PATH}}` / `{{INTENT_PROFILE_PATH}}` | 의도 원장(자동 의도 프로필) 파일 (없으면 해당 줄 삭제) | — |
| `{{CONFIDENTIAL_PROFILE_DOC}}` | 기밀 범위·등급 정의 문서 | `{{CONFIDENTIAL_DIR}}/...` |
| `{{CORE_PROJECT}}` | 기술 선택 정책이 적용되는 핵심 프로젝트명 | — |
| `{{AGENT_MEMORY_DIR}}` | 도구별 영속 메모리 폴더 | 도구 기본값 |
| `{{DIARY_ROLLOVER_DOC}}` / `{{ROSTER_DOC}}` / `{{RELAY_PROTOCOL_DOC}}` | 일지 롤오버 절차 / 로스터 정의 / 릴레이 프로토콜 정본 문서 (없으면 해당 줄 삭제) | — |

> 위 표에 없는 `{{...}}` 도 이름이 곧 의미입니다. 에디터 전역 검색 `{{` 으로 전부 찾아 치환하거나, 해당 기능을 안 쓰면 그 줄을 삭제하십시오.

## 치환 후 점검 3줄

```
1. {{ 가 남아 있는 파일이 없는가?   (전역 검색: "{{")
2. 17개 파일 frontmatter에 visibility 필드가 다 있는가?
3. 부팅 필수 2문서(헌법+매니페스트) 합계가 15KB 이하인가?
```
