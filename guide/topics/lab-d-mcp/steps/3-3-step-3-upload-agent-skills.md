이 실습에서 Agent Skills는 이미 업로드가 되어 있습니다. 사용된 Skill.md 파일들은 [GitHub Repository](https://github.com/swannekim/mcp-curator-servers/tree/main/docs/skills)에서 확인할 수 있습니다.

### Copilot Studio의 새로운 Skill 기능 요약

#### 1. Skill이란?

Copilot Studio의 **Skill**은 New Orchestrator가 필요할 때 불러와 사용하는 **재사용 가능한 작업 playbook**입니다.

기존에는 agent의 top-level instructions에 모든 규칙, tool 사용 방식, 예외 처리, workflow를 길게 작성해야 했습니다.
Skill을 사용하면 특정 업무나 시나리오별로 다음 내용을 별도 Markdown 기반 instruction으로 분리할 수 있습니다.

* 언제 이 Skill을 사용할지
* 어떤 tools / MCP servers를 사용할지
* 어떤 순서로 실행할지
* 입력이 부족할 때 어떻게 처리할지
* 최종 답변은 어떤 형식으로 반환할지
* 어떤 guardrails를 지켜야 할지

즉, Skill은 agent에게 “이런 요청이 오면 이 절차대로 처리해”라고 알려주는 **task-specific orchestration guide**로 볼 수 있습니다.

---

#### 2. 왜 중요한가?

이번 Copilot Studio 업데이트의 핵심은 단순 UI 변경이 아니라, agent가 더 복잡한 multi-step task를 처리할 수 있도록 **orchestration 중심 구조**가 강화되었다는 점입니다.

New Orchestrator는 사용자의 요청을 보고 다음을 판단합니다.

* 어떤 Skill이 적절한지
* 어떤 tool이나 MCP server를 호출해야 하는지
* tool 결과를 다음 단계 입력으로 어떻게 넘길지
* 최종적으로 어떤 응답을 만들어야 하는지

따라서 Skill을 잘 작성하면 agent의 동작을 더 일관되게 만들 수 있고, 복잡한 tool chaining도 하나의 재사용 가능한 패턴으로 관리할 수 있습니다.

---

#### 3. Copilot Studio Skill과 기존 구성 요소의 관계

Skill은 Topic, Tool, Connector, MCP server, Knowledge source를 완전히 대체한다기보다는, 이들을 **어떤 상황에서 어떤 순서로 사용할지 정의하는 orchestration layer**에 가깝습니다.

| 구성 요소                     | 역할                                                    |
| ------------------------- | ----------------------------------------------------- |
| Agent instructions        | agent 전체의 기본 역할과 전반적인 행동 규칙                           |
| Skill                     | 특정 업무를 위한 trigger, workflow, tool sequence, guardrail |
| Tool / Connector          | 실제 외부 시스템 호출 또는 action 수행                             |
| MCP Server                | agent가 사용할 수 있는 외부 tools/resources 제공                 |
| Knowledge source          | 답변 grounding을 위한 문서/지식                                |
| Workflow / Power Automate | deterministic process automation이 필요한 절차 실행           |

즉, Skill은 “실행 도구” 자체라기보다는, agent가 여러 tools와 MCP servers를 올바르게 조합해 사용하도록 안내하는 구조입니다.

---

#### 4. 이 Agent에서 사용한 Skill 구조

이번 Curator MCP Agent에서는 두 개의 Skill을 사용했습니다.

1. `museum-of-universe`
2. `art-doppelganger`

두 Skill 모두 공통적으로 다음 구성 요소를 포함합니다.

---

#### 5. Skill 파일의 주요 구성 요소

##### 1. YAML front matter

Skill 파일 상단에는 `name`과 `description`이 있습니다.

```yaml
---
name: museum-of-universe
description: Use this skill when the user wants a 3–5 piece mini exhibition...
---
```

이 부분은 orchestrator가 Skill을 식별하고, 어떤 요청에서 이 Skill을 load해야 하는지 판단하는 데 사용됩니다.

---

##### 2. Skill identity / role

Skill이 어떤 역할을 수행하는지 정의합니다.

예를 들어 `Museum of the Universe Curator`는 사용자의 theme을 받아 NASA space imagery와 public-domain artworks를 pairing하여 mini exhibition을 만드는 역할을 합니다.

`AI Curator: Art Doppelgänger`는 사용자가 제공한 personality words와 preferred colours를 기반으로 Art Institute of Chicago collection에서 어울리는 artwork를 찾아주는 역할을 합니다.

---

##### 3. When to use this skill

이 Skill을 언제 사용할지 정의하는 trigger 영역입니다.

예를 들어 `museum-of-universe` Skill은 다음 요청에 사용됩니다.

* mini exhibition
* universe museum
* NASA/space imagery paired with artworks
* themed art-and-space curation
* exhibition map 또는 Mermaid diagram

반면 `art-doppelganger` Skill은 다음 요청에 사용됩니다.

* art doppelgänger
* “what artwork am I?”
* personality-to-artwork match
* “my vibe as a painting”

이 부분이 명확해야 orchestrator가 잘못된 Skill을 선택하지 않습니다.

---

##### 4. Required tools

Skill이 사용할 MCP tools와 호출 순서를 정의합니다.

`museum-of-universe` Skill은 다음 순서로 tools를 사용합니다.

1. `NASA Space Data MCP`
2. `Art Institute Public Artwork MCP`
3. `Curator Scoring MCP`
4. `Mermaid Chart Rendering MCP`

`art-doppelganger` Skill은 NASA tool을 사용하지 않고, artwork search, personality ranking, Mermaid rendering 중심으로 tools를 사용합니다.

이처럼 Skill 안에 tool sequence를 명시하면 multi-step tool chaining이 더 안정적으로 동작합니다.

---

##### 5. Input handling

사용자의 입력이 부족하거나 불완전할 때 어떻게 처리할지 정의합니다.

예를 들어 `museum-of-universe` Skill은 사용자가 theme을 주지 않으면 짧은 theme 하나만 요청하고, 작품 수를 지정하지 않으면 기본값으로 5개를 사용합니다.

`art-doppelganger` Skill은 사용자가 3–5개의 personality words를 주지 않으면 이를 요청하고, 문장으로 입력한 경우 agent가 직접 3–5개의 personality words를 추출하도록 합니다.

---

##### 6. Process

Skill의 핵심 workflow입니다.

여기에는 agent가 어떤 순서로 reasoning하고 tools를 호출해야 하는지가 단계별로 정의됩니다.

예를 들어 `museum-of-universe` Skill의 process는 다음과 같습니다.

1. exhibition theme 식별
2. NASA image 검색
3. Art Institute artwork 검색
4. image가 없는 artwork 제거
5. space image와 artwork pairing
6. exhibition title, curator statement, wall label 생성
7. Mermaid exhibition map 생성
8. Mermaid rendering 수행

이 구조 덕분에 agent는 매번 비슷한 품질과 순서로 결과를 생성할 수 있습니다.

---

##### 7. Final response format

최종 응답 형식을 정의합니다.

`museum-of-universe` Skill은 다음을 포함하도록 설계되었습니다.

* exhibition title
* curator statement
* NASA image와 artwork pairing list
* wall label
* match score
* source links
* rendered exhibition-map link

`art-doppelganger` Skill은 다음을 포함합니다.

* best-match artwork
* match score
* personality/color 기반 explanation
* runner-ups
* rendered diagram link
* artwork source links

이 부분은 live demo에서 결과를 보기 좋게 만들고, agent 응답의 일관성을 높여줍니다.

---

##### 8. Style rules

응답 톤과 표현 방식을 정의합니다.

이번 agent에서는 museum-style demo에 맞게 다음과 같은 스타일을 사용했습니다.

* poetic
* concise
* museum-like
* playful but elegant
* creative curation으로 표현
* scoring을 객관적 진실처럼 말하지 않기

특히 artwork match나 exhibition pairing은 “scientific result”가 아니라 creative curation으로 다루도록 guardrail을 두었습니다.

---

##### 9. Error handling

tool 결과가 부족하거나 rendering이 실패했을 때의 fallback behavior를 정의합니다.

예를 들어:

* NASA images가 부족하면 가능한 image만으로 exhibition size 축소
* artwork search 결과가 부족하면 mood term을 한 번 broaden
* Mermaid rendering이 실패하면 diagram link 없이 결과 제공
* source link가 없으면 tool이 제공한 link만 포함

이 부분은 demo 중 tool 결과가 예상과 달라도 agent가 자연스럽게 대응하도록 돕습니다.

---

#### 6. 이번 Curator MCP Agent에서의 Skill 설계 의미

이번 agent는 하나의 top-level agent가 두 가지 Skill을 route하는 구조입니다.

| Skill                | 사용 목적                                               | 주요 MCP tools                                                                   |
| -------------------- | --------------------------------------------------- | ------------------------------------------------------------------------------ |
| `museum-of-universe` | space imagery와 artwork를 pairing한 mini exhibition 생성 | NASA Space Data MCP, Art Institute MCP, Curator Scoring MCP, Mermaid Chart MCP |
| `art-doppelganger`   | personality words 기반 artwork match                  | Art Institute MCP, Curator Scoring MCP, Mermaid Chart MCP                      |

Agent의 top-level instruction은 사용자의 요청을 보고 어떤 Skill을 사용할지 판단합니다.
각 Skill은 실제 업무 처리 방식, tool 호출 순서, 응답 형식, 예외 처리를 담당합니다.

즉, 전체 구조는 다음과 같이 이해할 수 있습니다.

```text
User request
  ↓
Top-level Agent Instructions
  ↓
Skill Routing
  ↓
Selected Skill
  ↓
MCP Tool Chaining
  ↓
Final curated response
```

---
