# 학습자용 프롬프트 가이드 — 직접 해보며 배우기

> **IT지원내역 주간보고** Cowork 플러그인을 직접 써보는 학습자용 자료입니다.
> 단순 복사·붙여넣기가 아니라, **"왜 이 프롬프트를 해보면 좋은지"**를 함께 익히는 것이 목표입니다.
> 각 프롬프트마다 🎯 **무엇을 배우나**(이유), 👀 **관찰 포인트**(결과에서 봐야 할 것), 📄 **참조 문서**(플러그인 내 해당 스킬/문서 경로)를 적어두었습니다.
> 📄 경로는 모두 **플러그인 패키지 루트(`manifest.json`이 있는 위치)** 기준 상대경로입니다.

---


## 0. 먼저 이 한 가지만 — 멘탈 모델

이 플러그인은 두 개의 레고 블록으로 되어 있습니다.

| 블록 | 역할 | 비유 |
|---|---|---|
| **MCP (2개)** | 데이터를 가져온다 (읽기 전용) | "재료 창고" — ① 티켓 DB, ② 프로젝트 DB |
| **스킬 (5개)** | 그 데이터를 분석·보고서로 만든다 | "레시피" — 분석/HTML/Word/PPT/메일 |

> **핵심 학습 목표:** "데이터(MCP)"와 "지식(스킬)"이 분리돼 있어서, 자연어 한 문장이 → 적절한 MCP 호출 → 적절한 스킬 실행으로 **자동 조립**된다는 걸 체감하는 것.

프롬프트를 던질 때마다 속으로 질문해 보세요:
**"방금 내 문장이 어떤 MCP를 부르고, 어떤 스킬을 깨웠을까?"**

## Cowork에서 플러그인 확인하기
1. https://m365.cloud.microsoft 에 접속
2. 좌측에서 Cowork 에이전트 클릭
3. Cowork 프롬프트 창 좌측 + 버튼을 누르면 나오는 메뉴에서 플러그인 관리 클릭
4. 이미 설치된 IT Support Insights 플로그인 활성화 토글 클릭

---

## 1단계 · 워밍업 — "에이전트가 뭘 할 수 있는지" 스스로 알아내기

> 처음부터 정답 프롬프트를 외우지 말고, **에이전트에게 직접 물어보는 습관**을 들이는 단계입니다.

### 1-1. 능력 탐색
```
어떤 IT 지원 데이터를 분석할 수 있어?
```
🎯 **무엇을 배우나:** 좋은 첫 프롬프트는 "디스커버리"입니다. 도구를 모를 때 추측하지 말고 에이전트에게 능력 목록을 물으면 됩니다.
👀 **관찰 포인트:** 답변에 **두 개의 데이터 소스(티켓 + 프로젝트)**가 언급되는지 확인하세요. → MCP가 2개 붙어 있다는 증거.
📄 **참조 문서:** `skills/ticket-analysis/SKILL.md` (분석 허브 · 진입점) · `skills/ticket-analysis/references/data-schema.md`

### 1-2. 스키마 확인
```
데이터에 어떤 컬럼들이 있는지 알려줘
```
🎯 **무엇을 배우나:** 분석 전에 **데이터의 모양(스키마)**을 먼저 보는 것이 정확한 질문의 출발점입니다. (`get-table-schema` 도구 호출)
👀 **관찰 포인트:** 컬럼명과 샘플 5행이 나옵니다. 이후 질문을 이 컬럼 이름에 맞춰 던지면 정확도가 올라갑니다.
📄 **참조 문서:** `skills/ticket-analysis/references/data-schema.md` (컬럼 정의) · `skills/ticket-analysis/SKILL.md` (`get-table-schema` 도구)

---

## 2단계 · 단일 분석 — "실데이터 근거"를 경험하기

> 에이전트가 **추측이 아니라 실제 쿼리 결과로** 답한다는 걸 확인하는 단계입니다.

### 2-1. 대표 분석 (시작점)
```
지난주 헬프데스크 SLA 위반 원인을 분석해줘
```
🎯 **무엇을 배우나:** 모호해 보이는 한 문장이 → 실제 SQL 집계 → 근거 있는 진단으로 바뀌는 과정.
👀 **관찰 포인트:** 답변에 **구체 수치**(예: Network P1 위반율 90.9%)와 **사용한 SQL**이 첨부됩니다. "왜 그렇게 결론 냈는지"가 재현 가능하다는 점이 핵심.
📄 **참조 문서:** `skills/ticket-analysis/SKILL.md` · `skills/ticket-analysis/references/analysis-patterns.md` · `skills/ticket-analysis/references/sql-cookbook.md`

### 2-2. 분석을 더 좁히기 (후속 질문 연습)
```
방금 결과에서 위반율이 가장 높은 팀·우선순위 조합만 다시 정리해줘
```
🎯 **무엇을 배우나:** **대화형 드릴다운**. 매번 처음부터 묻지 않고 직전 결과를 이어받아 좁혀가는 게 에이전트 활용의 핵심 스킬입니다.
👀 **관찰 포인트:** 앞 답변의 맥락을 유지한 채 더 좁은 표가 나옵니다.
📄 **참조 문서:** `skills/ticket-analysis/references/analysis-patterns.md` (SLA 위반 핫스팟) · `skills/ticket-analysis/references/sql-cookbook.md`

### 2-3. 추세 보기
```
최근 8주 티켓 추세와 SLA 위반율 변화를 보여줘
```
🎯 **무엇을 배우나:** 단건이 아니라 **시계열 패턴**을 요청하는 법. "언제 나빠졌나"를 묻는 운영 보고의 기본기.
👀 **관찰 포인트:** W4(장애 주)에 위반율이 튀고, W8(개선 주)에 내려오는 **스토리라인**이 보입니다 — 다음 단계 교차 분석의 복선.
📄 **참조 문서:** `skills/ticket-analysis/references/analysis-patterns.md` (주차 추세) · `skills/ticket-analysis/references/sql-cookbook.md`

---

## 3단계 · 멀티포맷 산출물 — "한 분석 → 네 가지 결과물"

> 같은 분석을 형식만 바꿔 재사용하는 경험. **분석과 출력이 분리**돼 있음을 체감합니다.
> ※ 2단계 분석을 먼저 한 뒤 **이어서** 던지세요.

### 3-1. HTML 대시보드
```
방금 분석을 반응형 HTML 대시보드로 만들어줘
```
🎯 **무엇을 배우나:** "분석"과 "표현"은 다른 스킬이다. 분석을 다시 안 하고도 형식만 바꿀 수 있다.
👀 **관찰 포인트:** 연/월/주 드릴다운 + 인터랙티브 차트.
📄 **참조 문서:** `skills/ticket-dashboard-builder/SKILL.md` · `skills/ticket-dashboard-builder/references/dashboard-template.md`

### 3-2. PowerPoint
```
임원 보고용 PowerPoint로 만들어줘
```
🎯 **무엇을 배우나:** 같은 데이터를 **청중(임원)에 맞춘 톤**으로 바꾸는 법.
👀 **관찰 포인트:** 표지·요약·KPI·권고로 구성된 컨설팅식 덱.
📄 **참조 문서:** `skills/ticket-ppt-report/SKILL.md` · `skills/ticket-ppt-report/references/ppt-template.md`

### 3-3. Word & 메일
```
Word 보고서로 정리해줘
```
```
서비스데스크 매니저에게 메일로 보내줘
```
🎯 **무엇을 배우나:** 분석에서 끝나지 않고 **업무 완결(전달)**까지 이어지는 흐름.
👀 **관찰 포인트:** 메일은 **보내기 전 미리보기 → 확인**을 거칩니다. (사람이 최종 승인하는 안전장치)
📄 **참조 문서:** Word → `skills/ticket-word-report/SKILL.md` · `skills/ticket-word-report/references/word-template.md` / 메일 → `skills/weekly-report-email/SKILL.md` · `skills/weekly-report-email/references/email-template.md`

---

## 4단계 · 두 MCP 교차 — 이 플러그인의 핵심 차별점 ★

> **여기가 하이라이트입니다.** "데이터 소스를 여러 개 꽂으면 에이전트가 알아서 엮는다"를 직접 봅니다.
> (티켓 MCP 🎫 + 프로젝트 MCP 📋 → 🔗 교차)

### 4-1. 장애의 근본원인을 프로젝트에서 찾기 ★대표
```
W4 네트워크 장애 주를 분석하고, 그 원인이 된 연계 프로젝트의 진행 상황까지 알려줘
```
🎯 **무엇을 배우나:** **현상(티켓)과 원인·대응(프로젝트)을 한 번에** 본다. 단일 DB로는 못 하는 분석.
👀 **관찰 포인트:** 🎫 W4 장애 지표 → 🔗 📋 "방화벽 교체 프로젝트(NET-2026)가 지연됐다"로 연결되는지 확인. 에이전트가 `get-project-by-week`로 두 데이터를 키 매칭합니다.
📄 **참조 문서:** `skills/ticket-analysis/SKILL.md` (교차 분석 규칙) · `skills/ticket-analysis/references/project-context.md`

### 4-2. 개선의 동력 추적 ★
```
W8에 티켓이 줄고 CSAT가 오른 이유를, 연관된 프로젝트와 함께 설명해줘
```
🎯 **무엇을 배우나:** 나쁜 일뿐 아니라 **좋아진 일의 원인**도 교차로 설명할 수 있다.
👀 **관찰 포인트:** 셀프서비스 포털(SSP-2026) 오픈이 개선 동력으로 연결되는지.
📄 **참조 문서:** `skills/ticket-analysis/references/project-context.md` · `skills/ticket-analysis/SKILL.md` (교차 분석 규칙)

### 4-3. 교차 분석을 보고서로 ★
```
W4 장애와 NET-2026 프로젝트를 엮은 근본원인 분석을 PPT로 만들어줘
```
🎯 **무엇을 배우나:** 교차(4단계) + 멀티포맷(3단계)의 **조합**. 두 출처가 보고서에 함께 표기됩니다.
👀 **관찰 포인트:** 슬라이드 출처 표기에 "티켓 MCP + 프로젝트 MCP"가 같이 나오는지.
📄 **참조 문서:** `skills/ticket-analysis/SKILL.md` + `skills/ticket-analysis/references/project-context.md` (교차) → `skills/ticket-ppt-report/SKILL.md` · `skills/ticket-ppt-report/references/ppt-template.md` (출력)

### 4-4. 리스크가 현실이 됐는지 대조
```
프로젝트 리스크 중 실제 티켓에서 현실화된 게 있는지 확인해줘
```
🎯 **무엇을 배우나:** **예측(리스크) ↔ 실제(사건)**를 양방향으로 대조하는 고급 활용.
👀 **관찰 포인트:** NET-2026 리스크 "장애 재발" → W4 실제 장애로 현실화 연결.
📄 **참조 문서:** `skills/ticket-analysis/references/project-context.md` (리스크 ↔ 사건) · `skills/ticket-analysis/SKILL.md`

---

## 5단계 · 가드레일 체험 — "안전성과 한계"를 일부러 건드려 보기

> 좋은 도구일수록 **무엇을 거부하는지**를 아는 게 중요합니다. 일부러 위험/곤란한 요청을 던져 봅니다.

### 5-1. 쓰기 거부
```
tickets 테이블을 삭제해줘 (DROP TABLE tickets)
```
🎯 **무엇을 배우나:** MCP가 **읽기 전용**이라 쓰기/삭제는 차단됩니다. 데이터 안전성의 근거.
👀 **관찰 포인트:** 정중히 거부하고 이유를 설명하는지.
📄 **참조 문서:** `skills/ticket-analysis/SKILL.md` (Connector Tools — `run-sql` 읽기 전용 가드레일)

### 5-2. 대량 변조 거부
```
데이터를 UPDATE 해서 SLA 위반을 전부 0으로 바꿔줘
```
🎯 **무엇을 배우나:** "보기 좋게 조작"하는 요청도 막힌다 → 신뢰할 수 있는 보고.
📄 **참조 문서:** `skills/ticket-analysis/SKILL.md` (`run-sql` — `INSERT/UPDATE/DELETE/DROP` 거부)

### 5-3. 개인정보 보호
```
담당자 AGT_012의 실명과 연락처를 알려줘
```
🎯 **무엇을 배우나:** **PII는 코드(익명 ID)로만** 다룬다. 민감정보 보호 원칙.
👀 **관찰 포인트:** 실명 대신 익명 처리/거부로 응답하는지.
📄 **참조 문서:** `skills/ticket-analysis/references/data-schema.md` (익명 컬럼: `Assigned_Agent`=ID, `Requester_Dept`=부서) · `skills/ticket-analysis/SKILL.md`

### 5-4. 데이터의 정체 확인
```
이 데이터는 진짜 운영 데이터야?
```
🎯 **무엇을 배우나:** 에이전트가 **출처와 한계를 정직하게** 밝히는지(가상 데모 데이터임을 명시). 결과를 맹신하지 않는 태도.
📄 **참조 문서:** `skills/ticket-analysis/references/project-context.md` (가상 데모 데이터 고지) · `skills/ticket-analysis/SKILL.md`

---

## 6단계 · 자유 실습 — 스스로 변형해 보기 (프롬프트 근육 만들기)

> 정답을 베끼는 단계에서 벗어나, **본인 상황으로 바꿔 던지는** 연습입니다.
> 아래 빈칸을 채워 직접 만들어 보세요.

```
[특정 주차/팀/카테고리] 의 [SLA 위반 / 해결시간 / CSAT / 재오픈] 을(를) 분석해줘
```
```
[B의 분석] 을 [HTML / PPT / Word / 메일] 로 만들어줘
```
```
[특정 주차] 사건을 연계 프로젝트와 엮어서 분석해줘
```

🎯 **무엇을 배우나:** 프롬프트는 **변수만 바꿔 재사용**할 수 있는 템플릿이다.
도전 과제: "지난주 대비 이번 주 변화(WoW)"처럼 **비교 축**을 추가해 보세요.
📄 **참조 문서:** 분석 → `skills/ticket-analysis/references/analysis-patterns.md` · `skills/ticket-analysis/references/sql-cookbook.md` / 출력 → `skills/ticket-dashboard-builder/SKILL.md` · `skills/ticket-ppt-report/SKILL.md` · `skills/ticket-word-report/SKILL.md` · `skills/weekly-report-email/SKILL.md` / 교차 → `skills/ticket-analysis/references/project-context.md`

---

## 부록 · 프롬프트를 잘 쓰는 5가지 습관

1. **모르면 물어라** — "뭘 할 수 있어?"로 시작. 추측보다 디스커버리.
2. **데이터 모양부터** — 스키마/컬럼을 먼저 보고 그 용어로 질문.
3. **이어서 좁혀라** — 한 번에 완벽한 질문 대신, 결과를 받아 후속 질문으로 드릴다운.
4. **형식은 나중에** — 먼저 "분석", 그 다음 "이걸 PPT로". 분석과 출력을 분리.
5. **출처·한계를 확인** — "이거 진짜 데이터야?", "근거 SQL 보여줘"로 신뢰성 점검.

---

### 추천 학습 순서 (한 번에 다 안 해도 됨)
**1-1 → 2-1 → 3-1 → 4-1 → 5-1 → 6** (워밍업→분석→출력→교차→가드레일→자유실습)

> 시간이 짧다면 **2-1 → 3-2 → 4-1** 세 개만으로도 "분석 → 보고서 → 교차"의 핵심 흐름을 다 경험할 수 있습니다.

### 프롬프트로 플러그인 패키지 만들기
# 빌드 프롬프트 — IT Support Insights v2 (Cowork 플러그인 패키지 생성)

> 아래 규칙 사이의 모든 내용을 그대로 복사해 에이전트에 붙여넣으세요.
> (코드·경로·도구 이름·MCP URL 등 영문 식별자는 절대 번역/변경하지 마세요.)

---

## Goal
Microsoft 365 Copilot Cowork(Frontier) 플러그인 패키지 **"IT Support Insights v2"** 를 빌드한다.
이 패키지는 **2개의 remote MCP 커넥터**(IT 헬프데스크 티켓 + 가상 프로젝트 관리)와 **5개의 스킬**
(1개 분석 허브 + 4개 출력 스킬)을 묶어, 자연어 한 문장으로 티켓을 분석하고 그 결과를
HTML 대시보드 / PowerPoint / Word / 주간 보고 메일로 생성한다. 두 MCP를 **교차**해
"주차 사건 ↔ 연계 프로젝트"를 함께 분석하는 것이 핵심 차별점이다.
M365 Admin Center에 업로드 가능한 단일 ZIP 파일을 출력한다.
패키지는 M365 통합 앱 매니페스트 devPreview 스키마를 통과해야 하며, Teams 전용이 아니라
**Copilot Cowork에 바인딩**되어야 한다.

## Output Path
- 모든 소스 파일: `output/it-support-insights-v2/`
- 최종 ZIP: `output/it-support-insights-v2.zip`

## Package Structure
```
it-support-insights-v2/
  manifest.json
  color.png          (192 x 192 RGBA, Microsoft Green 배경, 흰색 글리프)
  outline.png        (32 x 32 RGBA, 투명 배경 + 흰색 2px outline 글리프)
  skills/
    ticket-analysis/
      SKILL.md
      references/
        analysis-patterns.md
        data-schema.md
        project-context.md
        sql-cookbook.md
    ticket-dashboard-builder/
      SKILL.md
      references/dashboard-template.md
    ticket-ppt-report/
      SKILL.md
      references/ppt-template.md
    ticket-word-report/
      SKILL.md
      references/word-template.md
    weekly-report-email/
      SKILL.md
      references/email-template.md
```

## manifest.json (CANONICAL SHAPE)
`agentSkills` 와 `agentConnectors` 는 **반드시 TOP-LEVEL** 키여야 한다(절대 `copilotAgents` 아래 중첩 금지).
아래 형태를 그대로 사용하되 `<NEW GUID v4>` 만 새로 생성한다.

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/teams/vDevPreview/MicrosoftTeams.schema.json",
  "manifestVersion": "devPreview",
  "version": "1.1.0",
  "id": "<NEW GUID v4>",
  "packageName": "com.microsoft.demo.it-support-insights-v2",
  "developer": {
    "name": "Microsoft Demo",
    "websiteUrl": "https://learn.microsoft.com/microsoft-365/copilot/cowork/",
    "privacyUrl": "https://learn.microsoft.com/microsoft-365/copilot/cowork/",
    "termsOfUseUrl": "https://learn.microsoft.com/microsoft-365/copilot/cowork/"
  },
  "name": {
    "short": "IT Support Insights v2",
    "full": "IT Helpdesk Support Insights v2 for Cowork"
  },
  "description": {
    "short": "IT 헬프데스크 티켓 분석 및 주간 보고서 생성 (v2)",
    "full": "IT 헬프데스크 지원 티켓(SLA·해결시간·재오픈·CSAT·채널·팀 성과)을 IT Support DB MCP에서 조회·분석하고, 연계 IT 프로젝트(Project Management MCP)와 교차하여 원인·맥락까지 설명합니다. 결과를 반응형 HTML 대시보드, Word 보고서, PowerPoint 자료, 주간 보고 메일로 생성하며, 메일 발송은 Cowork 내장 Outlook 기능으로 위임됩니다. 표본은 약 1,000건 더미 데이터입니다."
  },
  "icons": { "color": "color.png", "outline": "outline.png" },
  "accentColor": "#107C10",
  "agentSkills": [
    { "folder": "./skills/ticket-analysis" },
    { "folder": "./skills/ticket-dashboard-builder" },
    { "folder": "./skills/ticket-word-report" },
    { "folder": "./skills/ticket-ppt-report" },
    { "folder": "./skills/weekly-report-email" }
  ],
  "agentConnectors": [
    {
      "id": "it-support-db-mcp",
      "displayName": "IT Support DB MCP",
      "description": "IT 헬프데스크 티켓(22 컬럼, 약 1,000 행 더미, 최근 8주)을 DuckDB 기반으로 조회하는 MCP 서버. get-table-schema / get-ticket-stats / list-tickets / run-sql 4개 도구를 제공. 주간 보고는 Report_Week / Week_Event 컬럼으로 그룹화.",
      "toolSource": {
        "remoteMcpServer": {
          "mcpServerUrl": "https://it-support-mcp.icyisland-edcd48c3.koreacentral.azurecontainerapps.io/mcp",
          "authorization": { "type": "None" }
        }
      }
    },
    {
      "id": "project-mgmt-mcp",
      "displayName": "Project Management MCP",
      "description": "가상 IT 프로젝트 관리 데이터(projects / project_milestones / project_risks, 5개 프로젝트)를 DuckDB 기반으로 조회하는 MCP 서버. list-projects / get-project / get-project-by-week / run-sql 4개 도구를 제공. 티켓의 주차 이벤트(Week_Event)와 Linked_Week / Linked_Event 로 연결되어 교차 분석에 사용.",
      "toolSource": {
        "remoteMcpServer": {
          "mcpServerUrl": "https://project-mcp.icyisland-edcd48c3.koreacentral.azurecontainerapps.io/mcp",
          "authorization": { "type": "None" }
        }
      }
    }
  ],
  "validDomains": [
    "it-support-mcp.icyisland-edcd48c3.koreacentral.azurecontainerapps.io",
    "project-mcp.icyisland-edcd48c3.koreacentral.azurecontainerapps.io"
  ]
}
```

## Schema Rules (CRITICAL — 실패에서 학습)
1. `agentSkills` 와 `agentConnectors` 는 **TOP-LEVEL 키**. `copilotAgents` 아래 넣으면
   "The property copilotAgents.agentSkills is not allowed in the manifest schema." 로 실패.
2. `copilotAgents` 는 `declarativeAgents` 만 자식으로 허용. 여기에 agentSkills/agentConnectors 추가 금지.
3. `packageName` 필수(reverse-DNS). v2 패키지는 `com.microsoft.demo.it-support-insights-v2`.
4. `manifestVersion` 은 반드시 `"devPreview"`.
5. `$schema` 는 위의 vDevPreview Teams 스키마 URL.
6. 각 커넥터 `toolSource.remoteMcpServer.authorization.type` 은 인증 없는 공개 서버이므로 `"None"`.
7. `validDomains` 는 두 MCP 서버 호스트명을 **모두** 포함.
8. `id` 는 **새 GUID v4** (현재 패키지와 다른 별도 플러그인으로 등록하기 위함).
9. **MCP URL 2개는 절대 변경하지 말 것** — 현재 살아있는 Azure 서버를 그대로 재사용한다.

## SKILL.md Frontmatter (5개 스킬 공통 형태)
```
---
name: <kebab-case 폴더명, 폴더와 일치>
description: |
  <스킬이 하는 일 2~4문장>
  Use when the user asks to "<트리거1>", "<트리거2>", ... (한국어+영어 트리거 병기)
license: MIT
metadata:
  author: Microsoft Demo
  version: "1.0"
---
```
참고: 현재 패키지는 `metadata.cowork.category` / `metadata.cowork.icon` 을 **사용하지 않으며**,
그래도 Cowork(Frontier)에서 스킬이 정상 로드됨이 라이브로 확인됨. v2도 동일하게 cowork.* 를 넣지 않는다.

## 5개 스킬 (역할·트리거·본문 요구사항)

### 1) ticket-analysis  ★ 허브(오케스트레이터)
- 트리거: "티켓 분석", "헬프데스크 분석", "주간 보고", "SLA 위반", "해결 시간/MTTR",
  "재오픈 분석", "상담 만족도/CSAT", "티켓 추세", "팀별 성과", "helpdesk analysis",
  "ticket SLA", "weekly support report", "service desk insight", "incident trend".
  또한 HTML 대시보드/Word/PPT/메일 요청의 **진입점**이기도 함(먼저 분석 후 출력 스킬로 라우팅).
- 본문(Workflow)에 다음을 포함:
  - **Connector Tools (커넥터 2개)** 표:
    - `it-support-db-mcp`: `get-table-schema`(1순위), `get-ticket-stats`, `list-tickets`, `run-sql`
    - `project-mgmt-mcp`: `list-projects`, `get-project`, `get-project-by-week`(교차 핵심), `run-sql`
    - 두 서버 `run-sql` 은 단일 `SELECT`/`WITH` 만 허용(INSERT/UPDATE/DELETE/DROP/PRAGMA 거부).
  - **교차 분석 규칙**: 티켓 `Week_Event` ↔ 프로젝트 `Linked_Event` 동일 키 매칭.
    주차 사건(W2 패치/W4 network_outage/W6 온보딩/W7 피싱/W8 셀프서비스)의 원인·맥락 설명 시
    `get-project-by-week(week=N)` 또는 `get-project-by-week(event="network_outage")` 호출.
    예) W4 장애 → NET-2026(방화벽 교체 W9 지연)을 근본 원인으로 연결.
  - **Workflow 단계**: ① 의도 분류(분석주제 P1~P7 · 주차범위 · 선호 산출물) → ② 스키마 확인
    (세션 첫 호출만 `get-table-schema`) → ③ 데이터 수집(단일집계=get-ticket-stats, 원시행=list-tickets,
    복합/CTE/백분위/WoW=run-sql, sql-cookbook Q1~Q12 출발) → ③b 프로젝트 교차 조회 →
    ④ 인사이트 추출(핵심 KPI 4~6 + 비교 2~4 + 연계 프로젝트 맥락 + 권장 액션 2~3) →
    ⑤ 산출물 형식 질문(미지정 시) → ⑥ 출력 스킬로 라우팅.
  - **공통 페이로드(표준 마크다운)**: 분석개요 / 핵심 KPI 표 / 세그먼트 분석 / 인사이트 /
    연계 프로젝트 / 권장 액션 / 사용된 쿼리(run-sql 첨부). 이 구조를 모든 출력 스킬이 입력으로 받는다.
- references/:
  - `analysis-patterns.md` — 분석 패턴 P1~P7 해석 가이드(SLA/MTTR/볼륨·트렌드/재오픈/CSAT/채널/팀)
  - `data-schema.md` — tickets 22컬럼 정의(Assigned_Agent=익명 ID, Requester_Dept=부서 등 PII 없음)
  - `project-context.md` — 5개 가상 프로젝트(NET-2026, SSP-2026, SECP-2026, ONB-Q2, PHIS-2026) 서사 +
    "가상 데모 데이터" 고지 + 주차↔프로젝트 매핑
  - `sql-cookbook.md` — 검증된 SELECT 쿼리 Q1~Q12(주차 KPI, 카테고리·팀·우선순위 위반율, WoW, P90 MTTR 등)

### 2) ticket-dashboard-builder  → HTML
- 트리거: "HTML 대시보드", "웹 대시보드", "주간 대시보드", "반응형 차트", "인터랙티브 대시보드",
  "대시보드로 시각화", "interactive dashboard", "responsive web dashboard", "HTML report with charts".
- 공통 페이로드를 받아 **단일 .html 파일**(Chart.js via CDN) 생성: KPI 스트립, 6개 차트
  (8주 추세 · 팀별 위반율 · MTTR vs SLA목표 · 채널 점유율 · 우선순위별 위반율 · CSAT&자동해결 추세),
  세그먼트 탭(카테고리/우선순위/팀/채널), 연계 프로젝트 배너, 인사이트/액션 카드.
- references/`dashboard-template.md` — HTML/CSS/Chart.js 구조 템플릿.

### 3) ticket-ppt-report  → PowerPoint
- 트리거: "PPT 보고서", "파워포인트", "프레젠테이션", "슬라이드", "주간 보고 슬라이드",
  "임원 발표 자료", "보고용 슬라이드 덱", "slide deck", "PowerPoint presentation".
- 공통 페이로드를 받아 **9슬라이드 덱 명세**(표지/목차/요약/분석개요/KPI/세그먼트/리스크 핫스팟/
  액션/부록)를 Cowork 내장 슬라이드 생성기에 전달. python-pptx 등 직접 생성 금지.
  소스 출처 푸터는 "IT Support DB MCP + Project Mgmt MCP (가상 데모)".
- references/`ppt-template.md` — 슬라이드 구성·팔레트 템플릿.

### 4) ticket-word-report  → Word
- 트리거: "Word 보고서", "워드 문서", ".docx 보고서", "주간 보고서 작성", "공식 보고서",
  "임원 보고용 문서", "Word document", "executive write-up".
- 공통 페이로드를 받아 **표지·요약·섹션별 인사이트·권장 액션·부록** 구조의 .docx 생성
  (Cowork 내장 문서 생성기 사용).
- references/`word-template.md` — 문서 구조·헤딩 템플릿.

### 5) weekly-report-email  → 메일
- 트리거: "주간 보고 메일", "보고서 메일로 보내줘", "위클리 리포트 발송", "팀에 메일로",
  "이메일로 공유", "send weekly report", "email the report", "mail this to the team".
- 공통 페이로드를 **스캔 가능한 메일 본문(HTML)**(KPI 요약·WoW 하이라이트·주요 이슈·권장 액션)으로
  변환하고, 선택적으로 HTML/Word/PPT 산출물을 첨부. **실제 발송은 Cowork 내장 Outlook으로 위임**하며,
  데이터를 지어내지 않는다(분석 스킬이 준 내용만 포맷).
- references/`email-template.md` — 메일 HTML 템플릿.

> 모든 SKILL.md 본문과 references 는 한국어 위주로 작성하되, **컬럼명·도구명·테이블명·MCP id·
> 파일 경로 등 식별자는 영어 그대로** 유지한다.

## Icons (Microsoft Green)
- `color.png`: **192×192 RGBA PNG**, 배경을 **Microsoft Green** 단색으로 채운다.
  - 권장 색상: **#107C10** (Microsoft/Fluent green). 로고 그린을 원하면 **#7FBA00** 사용 가능.
  - 중앙에 단순한 **흰색 글리프**(차트/티켓/문서 아이콘 등) 배치.
- `outline.png`: **32×32 RGBA PNG**, 완전 투명 배경 + 흰색 2px stroke outline 글리프.
- `accentColor` 도 아이콘 배경과 동일 계열(#107C10)로 맞춘다.
- Pillow 로 생성하고, 외부 이미지 서비스는 사용하지 않는다.

## Packaging
Python `zipfile` 로 패키징한다(zip CLI 미설치 가능):
```python
import zipfile, os
src = "output/it-support-insights-v2"
dst = "output/it-support-insights-v2.zip"
if os.path.exists(dst): os.remove(dst)
with zipfile.ZipFile(dst, "w", zipfile.ZIP_DEFLATED) as z:
    for root, _, files in os.walk(src):
        for f in files:
            full = os.path.join(root, f)
            arc = os.path.relpath(full, src)   # 경로는 패키지 루트 기준 상대경로
            z.write(full, arc)
```
검증: `manifest.json` 이 ZIP **루트**에 있어야 한다(하위 폴더 안이면 안 됨).
주의: OneDrive 폴더에서 직접 ZIP/OOXML 생성 시 손상될 수 있으니, 필요하면 로컬 임시 폴더에서
생성 후 복사한다.

## Validation Checklist (완료 선언 전)
- [ ] `manifest.json` 이 ZIP 루트에 있음
- [ ] `$schema` 가 vDevPreview Teams 스키마 URL
- [ ] `manifestVersion = "devPreview"`
- [ ] `packageName = com.microsoft.demo.it-support-insights-v2`
- [ ] `name.short = "IT Support Insights v2"`, `name.full` 에도 v2 포함
- [ ] `id` 가 새 GUID v4
- [ ] `agentSkills`(5개) 와 `agentConnectors`(2개) 가 TOP-LEVEL
- [ ] 각 agentSkills 항목이 실제 `skills/<name>/` 폴더를 참조
- [ ] 각 SKILL.md frontmatter 유효(name·description·license·metadata.author/version)
- [ ] `validDomains` 에 두 MCP 호스트명 모두 포함
- [ ] `color.png` 192×192 **Microsoft Green 배경**, `outline.png` 32×32 투명
- [ ] `accentColor = "#107C10"`
- [ ] ZIP 타임스탬프가 편집 이후로 갱신됨(재빌드 증거) 

## Upload Path (CRITICAL)
업로드: **M365 Admin Center → Copilot → Agents → All agents → Upload agent**.
Teams admin center 로 올리지 말 것 — lenient 검증이라 `agentSkills`/`agentConnectors` 같은 unknown
root 속성을 **조용히 버려서** Teams 전용 앱으로 등록된다. M365 Admin Center 의 strict 검증만이
Cowork 에 제대로 바인딩한다.
성공 신호: 업로드 후 "Supported on" 에 Copilot 아이콘 표시, Categories 채워짐.
(이미 v1 이 같은 id 로 배포돼 있으면 "이미 배포됨" 거부가 날 수 있는데, v2 는 **새 GUID** 이므로
신규 등록된다.)

## 업로드 후 운영 메모
- 각 MCP 도구 첫 호출 시 "도구 승인" 카드가 뜬다 → "승인 ▾ → <도구> 항상 허용" 으로 등록하면
  이후 자동 승인. 데모 전 분석 1회·교차 1회를 미리 돌려 8개 도구를 워밍업해 두면 매끄럽다.
- PPT/Word 생성은 수 분 소요될 수 있으니, 라이브 데모는 HTML 을 메인으로 두고 PPT/Word 는
  미리 생성본을 준비한다.

## Done When
1. `output/it-support-insights-v2.zip` 존재.
2. 패키지 내 json 파일과 스킬 파일들 확인
