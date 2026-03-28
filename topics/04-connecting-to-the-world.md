# How AI Connects to the World

_The integration layer: protocols, data retrieval, and how models reach external tools._

## Key Concepts

| Concept | Definition | Source |
|---------|-----------|--------|
| MCP (Model Context Protocol) | Open-source standard for connecting AI applications to external systems; like a USB-C port for AI | MCP Docs |
| MCP Host | The AI application (e.g., Claude Code, Claude Desktop) that manages connections to MCP servers | MCP Docs |
| MCP Client | Component inside the host that maintains a dedicated connection to one MCP server | MCP Docs |
| MCP Server | A program that exposes tools, resources, and/or prompts to AI applications via the MCP protocol | MCP Docs |
| MCP Tools | Executable functions the LLM can invoke to perform actions (search, create, modify, query) -- model-controlled | MCP Docs |
| MCP Resources | Read-only data sources that provide context (files, database schemas, API docs) -- application-controlled | MCP Docs |
| MCP Prompts | Reusable interaction templates that structure how the model works with tools and resources -- user-controlled | MCP Docs |
| JSON-RPC 2.0 | The underlying protocol MCP uses for client-server communication (requests, responses, notifications) | MCP Docs |
| Transport layer | How MCP messages travel: STDIO (local, same machine) or Streamable HTTP (remote, with auth) | MCP Docs |
| RAG | Retrieval Augmented Generation -- combining external knowledge retrieval with LLM generation for more accurate, grounded responses. The model doesn't "know" everything; RAG feeds it relevant docs at runtime | Glossary |
| MCP Connector | Feature that lets API users connect to MCP servers directly from the Messages API without building a separate MCP client | Glossary |

## MCP Architecture

```
┌──────────────────────────────────────────┐
│           MCP HOST (AI Application)      │
│          e.g., Claude Code, VS Code      │
│                                          │
│  ┌───────────┐  ┌───────────┐           │
│  │ MCP       │  │ MCP       │  ...      │
│  │ Client 1  │  │ Client 2  │           │
│  └─────┬─────┘  └─────┬─────┘           │
└────────│──────────────│──────────────────┘
         │              │
   dedicated       dedicated
   connection      connection
         │              │
   ┌─────▼─────┐  ┌─────▼─────┐
   │ MCP       │  │ MCP       │
   │ Server A  │  │ Server B  │
   │ (local)   │  │ (remote)  │
   │ e.g.      │  │ e.g.      │
   │ Filesystem│  │ Sentry    │
   └───────────┘  └───────────┘
```

| Participant | Role | Example |
|------------|------|---------|
| **MCP Host** | The AI application that manages everything | Claude Code, Claude Desktop, VS Code |
| **MCP Client** | Component inside the host; one per server connection | Created automatically when you add a server |
| **MCP Server** | Program that exposes capabilities (tools/resources/prompts) | Filesystem server, GitHub server, Sentry server |

One host connects to **many servers** simultaneously. Claude Code with 5 MCP servers = 5 MCP clients running, each with a dedicated connection.

## The Two Layers

| Layer | What it handles | Details |
|-------|----------------|---------|
| **Data layer** | Message structure and semantics | JSON-RPC 2.0. Lifecycle management, primitives, notifications |
| **Transport layer** | How messages travel | **STDIO** (local, no network) or **Streamable HTTP** (remote, with OAuth/API key) |

## The Three Primitives

| Primitive | What it is | Who controls it | Examples |
|-----------|-----------|----------------|---------|
| **Tools** | Executable functions the LLM can call | The **model** decides when | Search flights, send email, query database |
| **Resources** | Read-only data sources for context | The **application** manages | File contents, database schemas, API docs |
| **Prompts** | Reusable interaction templates | The **user** invokes them | "Plan a vacation", "Summarize meetings" |

**How they work together (travel booking example):**
1. **User** invokes a **prompt**: "Plan a vacation" with destination=Barcelona, budget=$3000
2. **Application** loads **resources**: calendar availability, travel preferences, past trips
3. **Model** calls **tools**: searchFlights(), checkWeather(), bookHotel()
4. Result: A fully booked trip assembled from multiple MCP servers

## Lifecycle: How a Connection Works

1. **Initialize:** Client sends capabilities, server responds with its capabilities
2. **Discover:** Client calls `tools/list`, `resources/list`, `prompts/list`
3. **Execute:** Client calls tools, reads resources, gets prompts as needed
4. **Notifications:** Server pushes updates without being asked
5. **Shutdown:** Connection terminates cleanly

## RAG (Retrieval Augmented Generation)

Instead of relying only on training data, you feed the model relevant documents at runtime. The model generates responses grounded in that retrieved evidence. Quality depends on what you retrieve. This is what happens when you paste a document or point Claude to a file.

## Open Standards for AI Agents

MCP is the most mature standard, but it only covers one problem: connecting agents to tools. Other layers of the agent stack don't have widely adopted standards yet.

### What exists

| Standard | Problem it solves | Who made it | Maturity |
|----------|------------------|-------------|----------|
| **MCP** | Agent-to-tool connection (data, APIs, services) | Anthropic | Adopted. Supported by Claude, Cursor, VS Code, OpenAI (adapting) |
| **CQ** | Agent-to-agent knowledge sharing (collective learning from errors) | Mozilla AI | Exploratory. Proof of concept |
| **A2A (Agent-to-Agent)** | Agent-to-agent communication across platforms | Google | Early stage |
| **OpenAI Agents SDK** | Framework for building multi-agent systems | OpenAI | Open source but OpenAI-centric |

### Gaps where no open standard exists

- **Agent memory.** No protocol for how agents should store, retrieve, and share memories. Claude Code uses prompt-engineered markdown files, CQ uses SQLite, other tools use vector databases. None are interoperable.
- **Agent identity and auth.** No standard for how an agent proves who it is to another agent or service. MCP handles tool auth (OAuth), but agent-to-agent trust is unsolved.
- **Knowledge portability.** If you switch from one AI coding tool to another, your agent's accumulated knowledge doesn't transfer.

### How agent memory works in practice

Claude Code's memory is not a database. It's markdown files on your filesystem managed by prompt instructions.

```
~/.claude/projects/{project-path}/memory/
├── MEMORY.md              ← Index file, always loaded into context at session start
├── feedback_*.md          ← Individual memory files
├── project_*.md
└── reference_*.md
```

The mechanism: system prompt includes rules about what to save and when. The model uses file-writing tools to create markdown files. Next session, the index loads automatically and individual files are read when they seem relevant. There's no ML, no embeddings, no vector search. "Remembering" = writing a file. "Recalling" = reading a file. The intelligence is in the prompt rules, not in infrastructure.

| Design choice | Strength | Weakness |
|--------------|----------|----------|
| Markdown files on disk | Readable, editable, transparent | No semantic search |
| Model decides when to save | Flexible, context-aware | Inconsistent |
| Index always loaded | Fast access | Caps at ~200 lines |
| Pull-based (model reads when relevant) | Low overhead | Misses things if model doesn't check |

**Compared to CQ's approach:**

| | Claude Code memory | CQ |
|---|---|---|
| **Storage** | Markdown files (filesystem) | SQLite database |
| **Query** | Model reads and pattern-matches in context | SQL queries (structured) |
| **Trigger** | Pull-based: model decides to read | Push-based: post-error hooks fire automatically |
| **Sharing** | Single user, single project | Local + team sync via REST API |
| **Portability** | Locked to Claude Code's project path | Open standard (Apache 2.0) |

The agent memory problem is where CQ and MCP could eventually converge: MCP defines how agents connect to tools, a future standard could define how agents connect to shared knowledge.
