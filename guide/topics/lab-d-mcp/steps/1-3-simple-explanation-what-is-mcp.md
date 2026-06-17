**MCP**는 **Model Context Protocol**의 약자입니다.

MCP는 AI agent가 외부 tools와 knowledge sources에 연결할 수 있게 해 주는 표준 방식이라고 생각하면 됩니다.

이 랩에서는 다음과 같이 이해하면 됩니다.

* **agent**는 Copilot Studio에서 생성됩니다.
* **MCP server**는 agent가 사용할 수 있는 tools 또는 정보를 제공합니다.
* agent는 사용자의 질문을 바탕으로 언제 해당 tools를 호출할지 결정합니다.

Microsoft Learn Docs MCP Server를 사용하면 agent가 Microsoft Learn 문서를 검색하고, Learn articles를 가져오고, Microsoft Learn의 code samples를 검색할 수 있습니다.

중요한 점은 다음과 같습니다.

* Copilot Studio는 **Streamable HTTP**를 통해 MCP를 지원합니다.
* SSE transport는 deprecated 되었으며, 2025년 8월 이후 Copilot Studio에서 지원되지 않습니다.

---
