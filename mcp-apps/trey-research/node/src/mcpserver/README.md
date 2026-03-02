# HR Consultant MCP Server

MCP server with rich Fluent UI React widgets for managing HR consultants, projects, and assignments.

## Prerequisites

| Requirement | Version |
|---|---|
| Node.js | ≥ 18 |
| npm | ≥ 9 |

Azurite is included as a dev dependency — no separate install needed.



## Set Up Dev Tunnel & Environment

1. **Create a dev tunnel** — Use [Dev Tunnels](https://learn.microsoft.com/azure/developer/dev-tunnels/) to expose your local MCP server publicly:

   ```bash
   devtunnel host -p 3001 --allow-anonymous
   ```

   Copy the forwarded URL (e.g. `https://<tunnel-id>.devtunnels.ms`).

2. **Create your `.env` file** — Inside `src/mcpserver`, copy the sample and update `SERVER_BASE_URL` with your dev tunnel URL:

   ```bash
   cp .env.sample .env
   ```

   Then edit `.env` and replace the placeholder with your actual tunnel URL:

   ```dotenv
   SERVER_BASE_URL=https://<your-tunnel-id>-3001.aue.devtunnels.ms/
   ```

   This URL is injected into the widgets so they can call back to the MCP server. Without it, widgets will default to `http://localhost:3001`.

## Quick Start

```bash
npm run install:all        # Install all dependencies
npm run start:azurite      # Start local Azure Table Storage (port 10002) (keep this running in a terminal)
npm run seed               # Seed consultants, projects, assignments (in a new terminal)
npm run build:widgets      # Build widget HTML into assets/
npm run start:server       # Start MCP server on http://localhost:3001
```

## Connect

For details on how to connect this MCP server to a Microsoft 365 Copilot Declarative Agent, see [Build declarative agents for Microsoft 365 Copilot with MCP](https://devblogs.microsoft.com/microsoft365dev/build-declarative-agents-for-microsoft-365-copilot-with-mcp/).

## MCP Tools

### Widget Tools

| Tool | Widget | Description |
|---|---|---|
| `show-hr-dashboard` | Dashboard | KPIs, consultant cards, project list. Optional filters: `consultantName`, `projectName`, `skill`, `role`, `billable`. |
| `show-consultant-profile` | Profile | Detailed profile card with contact info, skills, certifications, roles, and assignments. Requires `consultantId`. |
| `show-project-details` | Dashboard | Project detail with assigned consultants and forecasted hours. Requires `projectId`. |
| `search-consultants` | Bulk Editor | Search consultants by `skill` or `name`, results shown in the bulk editor for viewing and editing. |
| `show-bulk-editor` | Bulk Editor | View and edit consultant records. Optional filters: `skill`, `name`. |

### Data Tools

| Tool | Description |
|---|---|
| `update-consultant` | Update a single consultant's name, email, phone, skills, or roles. |
| `bulk-update-consultants` | Batch-update multiple consultant records at once. |
| `assign-consultant-to-project` | Assign a consultant to a project with a role, optional rate. |
| `bulk-assign-consultants` | Assign multiple consultants to a project at once. |
| `remove-assignment` | Remove a consultant's assignment from a project. |

## Sample Prompts

| Prompt | What it does |
|---|---|
| *Show me the HR dashboard* | Opens the dashboard widget with all data |


## Development

```bash
npm run dev:server         # Server with hot-reload (tsx --watch)
npm run build:widgets      # Rebuild widgets after changes
npm run inspector          # Launch MCP Inspector for testing
```
