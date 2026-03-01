# MCP Based Interactive UI Samples for Microsoft 365 Copilot

This repository contains sample MCP servers with rich interactive UI widgets that render inside Microsoft 365 Copilot. Use these samples to learn how to build declarative agents with visually rich tool responses.

## Interactive UI in Copilot

You can add interactive UI widgets to your declarative agents by adding a Model Context Protocol (MCP) server-based action to your agent and extending the MCP tools used by the agent to include UI. Microsoft 365 Copilot supports UI widgets created using the following methods:

- **[MCP Apps](https://modelcontextprotocol.github.io/ext-apps/api/documents/Overview.html)** — An extension to MCP that enables MCP servers to deliver interactive user interfaces to hosts.
- **[OpenAI Apps SDK](https://developers.openai.com/apps-sdk)** — Tools to build ChatGPT apps based on the MCP Apps standard with extra ChatGPT functionality.


## Samples

### Field Service Dispatch

MCP server for a field service dispatch workflow with assignment intake, map visualization, dispatch planning, and confirmation flow. Requires a free Mapbox token for map widgets.

- [OpenAI Apps SDK version](oai-apps-sdk/fieldops/node/README.md)

[![Watch demo video](oai-apps-sdk/fieldops/node/demos/screenshots/dispatchPlan_sbs_play.png)](https://www.youtube.com/watch?v=rsfPzTxCgjQ)

| Prompt | What it does |
|---|---|
| Show me new assignments from the last 24 hours. | Lists intake items in a list widget. |
| Show these assignments on the map. | Renders assignments on an interactive map. |
| Create a dispatch plan for these assignments. | Shows a dispatch planning UI with technician assignments. |

---

### Trey Research — HR Consultant Management

MCP server for managing HR consultants, projects, and assignments with Fluent UI React widgets including an HR dashboard, consultant profile cards, bulk editor, and project detail views.

- [MCP Apps version](mcp-apps/trey-research/node/src/mcpserver/README.md)
- [OpenAI Apps SDK version](oai-apps-sdk/trey-research/node/README.md)

![Trey Research demo](oai-apps-sdk/trey-research/node/assets/demo.png)

| Prompt | What it does |
|---|---|
| Show the HR dashboard. | Displays the full HR dashboard widget. |
| Show me the profile for consultant 1. | Renders a consultant profile card. |
| Open the bulk editor. | Opens the bulk assignment editor. |
| Show me project details for project 1. | Displays a project detail view. |

---

### Zava Insurance — Claims Management

MCP server for insurance claims management with claims dashboard, claim detail with map view, and contractor list widgets.

- [MCP Apps version](mcp-apps/zava-insurance/node/src/mcpserver/README.md)
- [OpenAI Apps SDK version](oai-apps-sdk/zava-insurance/node/README.md)

![Zava Insurance demo](oai-apps-sdk/zava-insurance/node/assets/demo.png)

| Prompt | What it does |
|---|---|
| Show me all insurance claims. | Displays the claims dashboard. |
| Show me claim CN202504990. | Renders a detailed claim view with map. |
| Show me all contractors. | Lists available contractors. |
| Approve claim 3 and add a note 'Verified by adjuster'. | Approves a claim with notes. |

---

### Employee Training

MCP server that recommends learning and training courses with embedded video previews, inline entity cards, and fullscreen course views.

- [MCP Apps version](mcp-apps/employee-training/node/README.md)

![Employee Training inline](mcp-apps/employee-training/node/demos/screenshots/learningmedia-inline.png)

| Prompt | What it does |
|---|---|
| Recommend a training course about AI agents. | Shows a course card with embedded video. |
| Show me a course on Semantic Kernel. | Renders the course widget with video player. |
| What training is available for Azure AI? | Returns a recommended course card. |

## Repository structure

```
mcp-apps/                        # MCP Apps SDK samples
  employee-training/node/        # Learning course recommendations
  trey-research/node/            # HR consultant management
  zava-insurance/node/           # Insurance claims management

oai-apps-sdk/                    # OpenAI Apps SDK samples
  fieldops/node/                 # Field service dispatch
  trey-research/node/            # HR consultant management (declarative agent)
  zava-insurance/node/           # Insurance claims management (declarative agent)

M365-Agents-Toolkit-Instructions.md   # Step-by-step guide for building agents
agents-toolkit-screenshots/           # Screenshots for the instructions
```