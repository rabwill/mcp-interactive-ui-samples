# Zava Insurance — MCP Server

An MCP (Model Context Protocol) server for **Zava Insurance** that exposes claims management tools and rich interactive widgets using [MCP Apps](https://github.com/modelcontextprotocol/ext-apps) (`@modelcontextprotocol/ext-apps`). Compatible with any MCP Apps-capable host such as Claude, ChatGPT, and Microsoft 365 Copilot.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Protocol | MCP SDK (`@modelcontextprotocol/sdk`) — `McpServer` high-level class |
| MCP Apps | `@modelcontextprotocol/ext-apps` — `registerAppTool`, `registerAppResource`, `RESOURCE_MIME_TYPE` |
| Transport | Express + `StreamableHTTPServerTransport` (stateless, JSON response) |
| Database | Azure Table Storage (`@azure/data-tables`) via Azurite local emulator |
| Widgets | React 18 + Fluent UI v9 + Vite (single-file HTML builds) |
| Theme | Reactive dark/light via MCP Apps `App.getHostContext()` |
| Language | TypeScript throughout |

## Tools

### Widget Tools (render interactive UI)

| Tool | Description |
|------|-------------|
| `show-claims-dashboard` | Grid view of all claims with status filters, metrics, and click-to-detail |
| `show-claim-detail` | Detailed view of a single claim with inspections, POs, and a map |
| `show-contractors` | Filterable list of contractors with ratings and specialties |

### Data Tools

| Tool | Description |
|------|-------------|
| `update-claim-status` | Update a claim's status and add notes |
| `update-inspection` | Update inspection status, findings, and recommended actions |
| `update-purchase-order` | Update a purchase order's status |
| `get-claim-summary` | Text summary of a specific claim |
| `list-inspectors` | List all inspectors with specializations |

## Quick Start

> **Note:** Run all commands from the root `mcpserver/` directory.

```bash
# 1. Install ALL dependencies (root + server + widgets)
#    This is required — each sub-project has its own package.json
npm run install:all

# 2. Start Azurite (local storage emulator) — run in a separate terminal
npm run start:azurite

# 3. Seed the database (requires Azurite to be running)
npm run seed

# 4. Build widgets
npm run build:widgets

# 5. Start the MCP server (port 3001)
npm run dev:server
```

The MCP server will be available at: `http://localhost:3001/mcp`

Connect from any MCP Apps-capable client by pointing it to `http://localhost:3001/mcp`.

For more on MCP Apps, see the [MCP Apps documentation](https://modelcontextprotocol.github.io/ext-apps/).

## Sample Prompts

| Prompt | What it does |
|--------|-------------|
| *Show me all insurance claims* | Opens the claims dashboard widget |
| *Show claims that are pending* | Dashboard filtered to pending claims |
| *Show me claim CN202504990* | Opens the detail view for that claim |
| *Approve claim 3 and add a note "Verified by adjuster"* | Updates claim status via `update-claim-status` |
| *Show me all contractors* | Opens the contractors list widget |
| *Show only preferred roofing contractors* | Filtered contractors list |
| *Mark inspection insp-005 as completed with findings "No structural damage found"* | Updates inspection |
| *Approve purchase order po-003* | Updates PO status |
| *Give me a summary of claim 7* | Returns a text summary |
| *List all inspectors* | Shows inspectors and their specializations |

## Project Structure

```
├── server/src/mcp-server.ts   # McpServer + registerAppTool / registerAppResource
├── server/src/index.ts        # Express entry point (Streamable HTTP transport)
├── server/src/db.ts           # Azure Table Storage data layer
├── widgets/src/
│   ├── claims-dashboard/      # Master-detail claims widget
│   ├── claim-detail/          # Standalone claim detail widget
│   ├── contractors-list/      # Contractors list widget
│   └── hooks/
│       ├── useMcpApp.tsx      # MCP Apps React context (App, useApp, theme)
│       └── useThemeColors.ts  # Color palette based on host theme
├── assets/                    # Built single-file HTML widgets
└── db/                        # Seed data (JSON)
```
