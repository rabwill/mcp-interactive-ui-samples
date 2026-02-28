# Employee Training — MCP App Server

A remote MCP App Server that recommends AI & Machine Learning training courses with embedded Microsoft Learn video previews. Built with React, Fluent UI v9, and the MCP Apps SDK.

## Features

- **AI Course Recommendation** — Accepts an optional topic query and randomly selects a matching course
- **Inline Entity Card** — Compact card with embedded video, metadata badges, and enroll button
- **Fullscreen View** — Expanded layout with video player, course modules sidebar, instructor info, and related courses
- **Display Mode Aware** — Adapts between inline and fullscreen based on host context
- **MS Learn Video Embeds** — Uses `learn-video.azurefd.net` for reliable video playback
- **Fluent UI v9 Theming** — Light/dark theme support via host context

## Available Courses

1. All About Azure AI Agents
2. Semantic Kernel's Agent Framework
3. Best Practices for Agentic Apps with Azure AI Foundry
4. Agentic Retrieval in Azure AI Search
5. Build Your Own Copilot with Azure AI Studio

## Quick Start

```bash
npm install
npm run build
npm run serve
```

The server starts on `http://localhost:3001/mcp` (Streamable HTTP, stateless).

## Architecture

```
main.ts          → Express + Streamable HTTP transport (stateless)
server.ts        → Tool & resource registration (registerToolWithUI helper)
mock-data/       → Course data with MS Learn video URLs
src/             → React UI (Vite + single-file bundle)
  training-media/App.tsx  → Inline card + fullscreen view
  shared/        → FluentWrapper, useToolData hook
ui/              → HTML entry points
build-ui.mjs     → Vite build script
```

## Tool: `training-media`

| Property | Value |
|---|---|
| Input | `query` (optional string) — topic keyword |
| Output | Text summary + `structuredContent` with single course |
| CSP | `frameDomains: ["https://learn-video.azurefd.net"]` |
| Annotation | `readOnlyHint: true` |

## Development

```bash
npm run dev   # Watch mode (Vite + tsx)
```

## Tunneling

For testing with Copilot, expose the server via ngrok:

```bash
ngrok http 3001
```

Then add the ngrok URL as an MCP server in your Copilot settings.
