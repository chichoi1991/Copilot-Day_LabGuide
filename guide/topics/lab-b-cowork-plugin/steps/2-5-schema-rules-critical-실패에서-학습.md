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
