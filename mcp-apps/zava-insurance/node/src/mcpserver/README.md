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

## Prerequisites

- [Node.js](https://nodejs.org/) (v18+)
- [Dev Tunnels CLI](https://learn.microsoft.com/azure/developer/dev-tunnels/get-started) — required to expose the local server to MCP Apps-capable hosts

## Quick Start

> **Note:** Run all commands from the root `src/mcpserver/` directory.

### 1. Create a Dev Tunnel

MCP Apps-capable hosts (Claude, ChatGPT, Microsoft 365 Copilot, etc.) need a publicly accessible URL to reach your local server. Use [Dev Tunnels](https://learn.microsoft.com/azure/developer/dev-tunnels/overview) to create one:

```bash
# Login (first time only)
devtunnel user login

# Create a persistent tunnel on port 3001 with anonymous access
devtunnel create --port 3001 --allow-anonymous
```

Copy the tunnel URL from the output (e.g. `https://<tunnel-id>.devtunnels.ms`). You'll need it in the next step.

> **Tip:** Keep the tunnel running in a separate terminal while developing.

### 2. Set up the `.env` file

Copy the sample environment file and update it with your tunnel URL:

```bash
cp .env.sample .env
```

Open `.env` and verify/update the values:

```dotenv
# Azure Table Storage (Azurite for local dev)
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;

# Server port
PORT=3001
```

### 3. Install, Build & Run

```bash
# Install ALL dependencies (root + server + widgets)
# This is required — each sub-project has its own package.json
npm run install:all

# Start Azurite (local storage emulator) — run in a separate terminal
npm run start:azurite

# Seed the database (requires Azurite to be running)
npm run seed

# Build widgets
npm run build:widgets

# Start the MCP server (port 3001)
npm run dev:server
```

### 4. Connect

The MCP server will be available locally at: `http://localhost:3001/mcp`

Use your **Dev Tunnel URL** to connect from any MCP Apps-capable client:

```
https://<tunnel-id>.devtunnels.ms/mcp
```

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
