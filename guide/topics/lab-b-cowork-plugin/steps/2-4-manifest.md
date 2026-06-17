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
