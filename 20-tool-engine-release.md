---
title: "OpenSIN Tool Engine: 42 Production-Ready Tools, API Server & CLI — Now Open Source"
date: 2026-04-04
author: OpenSIN Team
tags: [release, tool-engine, api, cli, open-source]
category: Engineering
---

# OpenSIN Tool Engine: 42 Production-Ready Tools, API Server & CLI

Today we're releasing the most comprehensive AI coding agent tool system ever built — **42 production-ready tools**, a full **API server** with OpenAI-compatible endpoints, and a **standalone CLI** that connects to it all.

## What's New

### 42 Tools (Claude Code Parity + More)

We've built every tool that Claude Code has, plus the unique OpenSIN features that no one else offers:

**Core Tools:** Bash (with sandbox), FileRead, FileEdit, FileWrite, Glob, Grep
**Web Tools:** WebFetch, WebSearch (DuckDuckGo, no API key)
**AI Tools:** Agent (sub-agent orchestration), Skill loading
**Productivity:** TodoWrite, Config, Sleep, StructuredOutput, NotebookEdit
**System:** PowerShell, REPL, KillShell, TaskOutput, GetErrors
**Workflow:** ScheduleCron, Plan Mode, Worktree, Brief, Switch Mode
**Team:** TeamCreate, TeamDelete, TeamUpdate, TeamMessage
**MCP:** McpAuth, ReadMcpResource, ListMcpResources
**Utility:** Help, Status, Version, RemoteTrigger, ToolSearch

### Full API Server

The OpenSIN API server provides OpenAI-compatible endpoints alongside powerful tool management:

- **Tool Engine** — Execute, search, and list all 42 tools
- **Session Management** — Create, resume, list, and delete sessions
- **MCP Integration** — Connect and manage MCP servers
- **Compaction** — Context summarization with auto-compact
- **Permissions** — Granular 5-tier permission system
- **OpenAI Compatible** — Drop-in `/v1/chat/completions` endpoint
- **Health Checks** — Basic, detailed, ready, and live probes
- **Rate Limiting** — Token bucket with API key tiers
- **Input Validation** — XSS and path traversal protection

### Standalone CLI

The OpenSIN CLI connects to the API server with three modes:

```bash
# Interactive REPL
opensin --api-url http://localhost:8000

# Single query
opensin "Read src/main.py" --api-url http://localhost:8000

# Pipe mode
echo "Find all Python files" | opensin --api-url http://localhost:8000
```

## Architecture

```
┌─────────────────────────────────────────────────────┐
│                    OpenSIN CLI                       │
│  ┌──────────┐  ┌─────────────────┐  ┌────────────┐  │
│  │   REPL   │  │ Session Manager │  │  History   │  │
│  └──────────┘  └─────────────────┘  └────────────┘  │
└──────────────────────┬──────────────────────────────┘
                       │ HTTP / SSE
┌──────────────────────▼──────────────────────────────┐
│                  API Server                          │
│  ┌────────┐ ┌─────────┐ ┌──────┐ ┌──────────────┐  │
│  │ Tools  │ │Sessions │ │ MCP  │ │  Compaction  │  │
│  └────────┘ └─────────┘ └──────┘ └──────────────┘  │
│  ┌──────────────────────────────────────────────┐   │
│  │          OpenAI /v1/chat/completions          │   │
│  └──────────────────────────────────────────────┘   │
└──────────────────────┬──────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────┐
│                  Tool Engine                         │
│  42 Tools: Bash, FileRead, FileEdit, FileWrite,     │
│  Glob, Grep, WebFetch, WebSearch, Agent, LSP, ...   │
└─────────────────────────────────────────────────────┘
```

## By The Numbers

| Metric | Value |
|---|---|
| **Tools** | 42 |
| **API Endpoints** | 24+ |
| **Tests** | 330 (Python) + 309 (TypeScript) |
| **Linting** | 0 errors |
| **Coverage** | 72% |
| **New Files** | 80+ |

## Getting Started

### 1. Start the API Server

```bash
cd opensin_core
pip install -e .
uvicorn opensin_core.api_server.server:app --reload --port 8000
```

### 2. Use the CLI

```bash
cd OpenSIN-Code/packages/opensin-sdk
npm install && npm run build && npm link
opensin --api-url http://localhost:8000
```

### 3. Or Use the API Directly

```bash
curl http://localhost:8000/api/tools/list
curl -X POST http://localhost:8000/api/tools/execute \
  -H "Content-Type: application/json" \
  -d '{"tool_name": "bash", "params": {"command": "echo hello"}, "workspace": ".", "session_id": "test", "permission_mode": "allow"}'
```

## What Makes OpenSIN Different

1. **Fully Open Source** — Apache 2.0 license, no black boxes
2. **42 Tools** — Complete Claude Code parity
3. **Multi-Provider** — OpenSIN, OpenAI, Anthropic support
4. **Swarm Integration** — Multi-agent orchestration built-in
5. **Voice** — Gemini S2S integration
6. **Cron** — Natural language scheduling
7. **Provider Routing** — AI-optimized multi-provider routing
8. **Fleet Dashboard** — Real-time monitoring

## Roadmap

- [ ] TUI (Terminal UI) with Vim mode
- [ ] Plugin system with hot-reload
- [ ] Team Memory Sync
- [ ] AutoDream background consolidation
- [ ] Analytics integration

## Links

- **GitHub:** [OpenSIN](https://github.com/OpenSIN-AI/OpenSIN)
- **CLI:** [OpenSIN-Code](https://github.com/OpenSIN-AI/OpenSIN-Code)
- **Docs:** [docs.opensin.ai](https://docs.opensin.ai)
- **API Reference:** [API Docs](https://docs.opensin.ai/api)

---

*OpenSIN is the most comprehensive open-source AI agent system in the world. Built by developers, for developers.*
