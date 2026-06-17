**각** server에 대해 다음 순서로 진행합니다. **Tools → Add a tool → New tool → Model Context Protocol**

| Server name         | Server URL                                                                            | Transport  | Auth |
| ------------------- | ------------------------------------------------------------------------------------- | ---------- | ---- |
| NASA Data MCP       | `https://mcp-curator-nasa-sage.kindmoss-321119b4.eastus.azurecontainerapps.io/mcp`    | Streamable | None |
| Art Institute MCP   | `https://mcp-curator-artic-sage.kindmoss-321119b4.eastus.azurecontainerapps.io/mcp`   | Streamable | None |
| Curator Scoring MCP | `https://mcp-curator-scoring-sage.kindmoss-321119b4.eastus.azurecontainerapps.io/mcp` | Streamable | None |
| Mermaid Chart MCP   | `https://mcp.mermaid.ai/mcp`                                                          | Streamable | None |

**Create**를 클릭합니다. Copilot Studio가 custom connector를 생성합니다. **create a connection** 메시지가 표시되면,
진행합니다. `"None"` auth인 경우 한 번 클릭하면 됩니다. 그러면 server의 tools가 표시됩니다. 예: `get_space_images_for_theme`,
`find_artworks_by_mood`, `pair_space_images_with_artworks` 등

**Server descriptions** (붙여넣기 — agent가 잘못된 server를 호출하지 않도록 도와줍니다):

- **NASA Data:**
    ```text
    Use only to retrieve NASA space imagery, astronomy descriptions, and space keywords. Not for artwork or diagrams.
    ```
    - NASA space imagery, astronomy descriptions, space keywords를 가져올 때만 사용하세요. artwork나 diagrams에는 사용하지 마세요.
- **Art Institute:**
    ```text
    Use only to search public-domain artworks and retrieve artwork metadata and IIIF image URLs.
    ```
    - public-domain artworks를 검색하고 artwork metadata와 IIIF image URLs를 가져올 때만 사용하세요.
- **Curator Scoring:**
    ```text
    Use only to pair space images with artworks, rank artworks for a personality, and generate Mermaid diagram syntax. Does no external lookups.
    ```
    - space images와 artworks를 매칭하고, personality에 맞는 artworks를 ranking하며, Mermaid diagram syntax를 생성할 때만 사용하세요. 외부 조회는 수행하지 않습니다.
- **Mermaid Chart:**
    ```text
    Use only to validate and render Mermaid diagram syntax into a PNG/SVG/playground link.
    ```
    - Mermaid diagram syntax를 validate하고 PNG/SVG/playground link로 render할 때만 사용하세요.

---
