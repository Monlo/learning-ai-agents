# How I Use AI Daily

_The practical layer: Claude Code, Agent Skills, and how everything fits together in a real workflow._

## Key Concepts

| Concept | Definition | Source |
|---------|-----------|--------|
| Agent Skills | Modular, reusable capabilities packaged as filesystem directories with instructions, code, and resources that extend Claude | Agent Skills Docs |
| Progressive disclosure | Skills load in stages: metadata at startup → instructions when triggered → resources as needed; minimizes context window usage | Agent Skills Docs |
| SKILL.md | Core file of an Agent Skill: YAML frontmatter (name, description) + markdown body with instructions Claude follows | Agent Skills Docs |
| Custom Agents | `.claude/agents/*.md` files that define specialized subagents with own name, color, tools, permissions, model, and preloaded skills | Best Practices Repo |
| Rules | Topic-specific instructions in `.claude/rules/*.md` with path-scoping -- more granular than CLAUDE.md | Best Practices Repo |
| Hooks | Deterministic scripts that run on specific Claude Code lifecycle events (PreToolUse, PostToolUse, Stop, SessionStart, etc.) | Best Practices Repo |
| Plugins | Distributable packages that bundle skills, agents, hooks, and MCP servers into a single installable unit | Best Practices Repo |
| Sandbox | File and network isolation runtime (`/sandbox`) that reduces permission prompts while keeping Claude contained | Best Practices Repo |
| Command → Agent → Skill pattern | Architecture where a slash command is the entry point, invokes an agent that orchestrates the workflow, with skills providing domain knowledge | Best Practices Repo |
| Context engineering | The practice of designing what information Claude sees and when -- CLAUDE.md structure, progressive disclosure, compaction timing | Best Practices Repo |
| Programmatic Tool Calling (PTC) | API feature where Claude writes Python code to orchestrate multiple tools internally; only final output enters context (~37% token savings) | Best Practices Repo |
| Tool Search | Auto-defers infrequently-used tool definitions until needed (~85% token savings); enabled by default in Claude Code v2.1.7+ | Best Practices Repo |

## Claude Code vs Claude API

```
You (natural language)
  └── Claude Code (the agentic shell)
        ├── reads/writes your files
        ├── runs bash commands
        ├── manages Agent Skills
        ├── connects to MCP Servers
        ├── spawns subagents
        │
        └── calls the Claude API underneath
              └── Claude (the model) thinks and responds
```

**Claude API** = the raw brain. You send a prompt, you get text back. Powerful but you build everything around it yourself.

**Claude Code** = a full agent wrapped around that brain. File access, tool use, skills, MCP, subagents, and a terminal interface -- all built in.

| | Claude API | Claude Code |
|---|---|---|
| **What it is** | Programmatic interface to send messages and get responses | CLI application built on top of the API with agentic capabilities |
| **How you use it** | Write code (Python, TypeScript) making HTTP requests | Type natural language in your terminal |
| **Who it's for** | Developers building AI into their own apps | Anyone who wants Claude as a hands-on project collaborator |
| **File access** | No -- only sees what you send it | Yes -- reads, writes, searches, edits files in your project |
| **Run commands** | No | Yes -- bash, git, scripts, etc. |
| **Agent Skills** | Supported (via code execution container) | Supported (filesystem-based custom skills) |
| **MCP Servers** | You build the integration yourself | Built-in: `claude mcp add` and it works |
| **Subagents** | You orchestrate them yourself with code | Built-in: Claude Code spawns subagents automatically |
| **Billing** | Pay per token (input + output) | Included in Claude Pro/Max subscription |

**When would you use the API directly?**
- Building a product or app that uses Claude (e.g., a chatbot for an organization)
- Creating custom multi-agent systems with the Agent SDK
- Integrating Claude into automated pipelines (CI/CD, data processing)
- When you need fine-grained control over every interaction

For daily work, Claude Code is the right tool. The API becomes relevant when building AI-powered products for others.

## Agent Skills

Skills are **filesystem-based packages** (a folder with a `SKILL.md` file + optional scripts/resources) that give Claude domain-specific expertise. Unlike prompts (one-off instructions), Skills are persistent, reusable, and load automatically when relevant.

**How Skills load (Progressive Disclosure):**

| Level | When loaded | Token cost | What it contains |
|-------|-----------|------------|-----------------|
| **1. Metadata** | Always (at startup) | ~100 tokens | `name` and `description` from YAML frontmatter |
| **2. Instructions** | When triggered | <5k tokens | SKILL.md body: workflows, best practices, guidance |
| **3. Resources** | As needed | Effectively unlimited | Bundled files: scripts, schemas, templates, reference docs |

**Skill structure:**

```yaml
---
name: your-skill-name
description: What it does and when to use it
---
# Your Skill Name
## Instructions
[Step-by-step guidance]
## Examples
[Concrete usage examples]
```

A Skill can bundle additional files (scripts are efficient: Claude runs them but their code never enters the context window -- only the output does):
```
my-skill/
├── SKILL.md          (main instructions)
├── REFERENCE.md      (detailed docs)
└── scripts/
    └── process.py    (executable code)
```

**Where Skills work:**

| Surface | Pre-built Skills | Custom Skills | Sharing |
|---------|-----------------|---------------|---------|
| **Claude.ai** | Yes | Yes (upload zip) | Individual user only |
| **Claude API** | Yes (via `skill_id`) | Yes (via `/v1/skills` endpoints) | Workspace-wide |
| **Claude Code** | No | Yes (filesystem-based) | Personal (`~/.claude/skills/`) or project (`.claude/skills/`) |
| **Agent SDK** | No | Yes (filesystem in `.claude/skills/`) | Via SDK config |

**Pre-built Skills available now:** PowerPoint, Excel, Word, PDF.

**Security:** Treat Skills like installing software -- audit all files before using third-party Skills.

## MCP in Claude Code

**Adding servers:**
```bash
# Remote HTTP server (recommended)
claude mcp add --transport http <name> <url>

# Local server
claude mcp add <name> -- <command> [args...]

# Import from Claude Desktop
claude mcp add-from-claude-desktop
```

**Managing servers:**
```bash
claude mcp list           # List all connected servers
claude mcp get <name>     # Details for one server
claude mcp remove <name>  # Remove a server
/mcp                      # Check status inside Claude Code
```

**Installation scopes:**
| Scope | Stored in | Shared? |
|-------|----------|---------|
| **Local** (default) | `~/.claude.json` | Private to you |
| **Project** | `.mcp.json` in project root | Shared with team via git |
| **User** | `~/.claude.json` | Available across all your projects |

**Authentication:** Remote servers use OAuth 2.0. Add the server, run `/mcp`, complete the browser flow.

**Useful MCP servers:** Sentry, GitHub, PostgreSQL, Filesystem, Figma.

## How Everything Fits Together

```
HOW CLAUDE IS BUILT:    Pretraining → Fine-tuning → RLHF + CAI → HHH model
HOW IT PROCESSES:       Text → Tokens → Context window → Output
HOW IT CONNECTS:        MCP (tools/data) + RAG (retrieved knowledge)
HOW IT'S MEASURED:      Latency, TTFT, token usage
HOW I USE IT:           Claude Code (host) + Skills (domain knowledge) + MCP (external access)
```

| Layer | What it provides | What I've learned |
|-------|-----------------|-------------------|
| **Claude (the model)** | Intelligence, reasoning, language | The "brain" -- built via pretraining + fine-tuning + RLHF + CAI |
| **Claude Code (the host)** | Terminal interface, file access, bash, subagents | The environment I work in daily |
| **Agent Skills** | Domain-specific knowledge and workflows | Custom skills for workflow automation, research, content editing, and more |
| **MCP Servers** | Connections to external tools, data, and APIs | The "arms and legs" -- how Claude reaches the outside world |

**Skills teach Claude *what to do*. MCP gives Claude *access to do it*. Together = specialized, connected agent.**

## Custom Agents (`.claude/agents/`)

Agents are more powerful than skills -- they get their own name, color, tool permissions, model, and can preload skills. Defined as `.md` files with YAML frontmatter:

```yaml
---
name: my-agent
description: When to invoke this agent
tools: Read, Write, Bash, Glob, Grep  # allowlist (inherits all if omitted)
model: haiku  # haiku, sonnet, opus, or inherit
permissionMode: acceptEdits  # or plan, bypassPermissions
skills: [skill-a, skill-b]  # preloaded at startup
color: blue  # CLI output color
background: true  # always run as background task
isolation: worktree  # temporary git worktree
memory: project  # user, project, or local
---
# Agent instructions here
```

**Key rule:** Subagents cannot invoke other subagents via bash. Use the Task tool instead.

## Command → Agent → Skill Architecture

The most powerful pattern for multi-step workflows:

| Component | Role | Example |
|-----------|------|---------|
| **Command** | Entry point, user interaction | `/weather-orchestrator` |
| **Agent** | Orchestrates workflow, preloads skills | `weather` agent |
| **Skills** | Domain knowledge injected at startup | `weather-fetcher`, `weather-transformer` |

**Why it works:** Progressive disclosure (skills load only when the agent needs them), single execution context (no context fragmentation), clean separation of concerns, reusability.

## Hooks System

Hooks are deterministic scripts that fire on Claude Code lifecycle events:

| Event | When it fires |
|-------|--------------|
| `PreToolUse` | Before a tool executes |
| `PostToolUse` | After a tool executes |
| `UserPromptSubmit` | When user sends a message |
| `Notification` | When Claude sends a notification |
| `Stop` | When Claude stops responding |
| `SubagentStart/Stop` | When subagents launch or finish |
| `PreCompact` | Before context compression |
| `SessionStart/End` | Session lifecycle |

Hooks live in `.claude/hooks/`. Can be used for sound notifications, logging, permission routing to Slack, or nudging Claude to continue turns.

## Rules (`.claude/rules/`)

More granular than CLAUDE.md -- topic-specific instructions with path-scoping:
- `.claude/rules/testing.md` -- applies only when working with test files
- `.claude/rules/api.md` -- applies only in API-related directories

## Boris Cherny's 12 Tips (from Claude Code's creator)

1. **`/config`** -- light/dark mode, notifications, `/terminal-setup` for shift+enter, `/vim` for vim mode
2. **`/model`** -- adjust effort: Low (fast), Medium (balanced), High (more tokens/intelligence). Creator prefers High.
3. **`/plugin`** -- install from official marketplace or create company-specific ones
4. **Custom agents** -- `.claude/agents/*.md` with names, colors, tool sets, permissions
5. **`/permissions`** -- wildcard allow/block lists: `Bash(npm run *)`, `Edit(/docs/**)`
6. **`/sandbox`** -- file and network isolation, fewer permission prompts
7. **`/statusline`** -- custom display showing model, directory, context, cost
8. **`/keybindings`** -- remap any key, settings reload live
9. **Hooks** -- deterministic lifecycle scripts (route permissions to Slack, logging, etc.)
10. **Spinner verbs** -- customize in `settings.json`, share with team
11. **Output styles** -- Explanatory (learning codebases), Learning (coaching), Custom
12. **Configuration hierarchy** -- codebase, sub-folder, personal, enterprise-wide. 37 settings + 84 env variables.

## Recommended MCP Servers for Daily Use

Community consensus: most people install 15+ MCP servers but only use 4-5 daily.

| Server | What it does | When to use |
|--------|-------------|-------------|
| **Context7** | Fetches current library docs, prevents hallucinated APIs from outdated training data | "By far the best MCP for coding" -- research phase |
| **Playwright** | Browser automation: screenshots, navigation, form testing | Essential for frontend development/testing |
| **Claude in Chrome** | Connects to live Chrome: console, network traffic, DOM inspection | "Game changer" for debugging real user experiences |
| **DeepWiki** | Structured wiki-style docs for any GitHub repo: architecture, API surfaces | Understanding unfamiliar codebases |
| **Excalidraw** | Generates architecture diagrams and flowcharts from natural language | Documentation and planning |

**Recommended workflow:** Research (Context7/DeepWiki) → Debug (Playwright/Chrome) → Document (Excalidraw)

## Advanced API Features (for reference)

| Feature | What it does | Token savings |
|---------|-------------|--------------|
| **Programmatic Tool Calling** | Claude writes Python to orchestrate multiple tools; only final output enters context | ~37% |
| **Dynamic Filtering** | Auto-filters web results to extract only relevant content | ~24% input tokens |
| **Tool Search** | Defers infrequently-used tool definitions until needed (auto in Claude Code v2.1.7+) | ~85% |
| **Tool Use Examples** | Add `input_examples` to tool schemas showing concrete usage patterns | 72% → 90% accuracy |

## Best Practices (Community-Tested)

**Workflow:**
- Keep CLAUDE.md **under 150 lines** for reliable adherence
- Use **commands for workflows** rather than standalone agents
- Create **feature-specific subagents with skills** for progressive disclosure
- Manual **`/compact` at ~50% context** usage
- **Always begin sessions in plan mode** for complex tasks
- Keep subtasks **small enough to complete in under 50% context**
- **Commit frequently** upon task completion
- Vanilla Claude Code outperforms complex workflows with fragmented tasks

**Utilities:**
- iTerm terminal preferred over IDE (stability issues in IDE terminals)
- Wispr Flow for voice prompting (reported 10x productivity increase)
- Git worktrees for parallel development
- `/doctor` for diagnostics when things aren't working
- Provide screenshots when reporting visual issues

## Tips & Tricks

- Skills must be directories with a `SKILL.md` file + YAML frontmatter -- not just `.md` files in the skills folder.
- Put skills in `~/.claude/skills/` (global) if you want them available from any directory.
- Claude Code launches from your current directory -- skills in `.claude/skills/` only work if that's where you run `claude` from.
- Always commit to git before letting Claude make big changes -- this gives you a safe rollback point.

## Auto Mode: AI Classifiers as Safety Layer

**Source:** [Claude Code auto mode](https://www.anthropic.com/engineering/claude-code-auto-mode) (March 2026)

The permission prompt problem: every file edit and bash command requires manual approval. Auto mode uses a second AI model (Sonnet 4.6) as an automated approver, replacing the human click with a classifier.

**Two-stage classification:**

```
Claude wants to run a command
  └── Stage 1: Fast single-token check (cheap, conservative)
        ├── Looks safe → execute
        └── Flagged → Stage 2: Chain-of-thought reasoning (expensive, nuanced)
              ├── Approved → execute
              └── Denied → block + tell Claude why
```

**Three tiers of trust:**

| Tier | What | Classifier? | Rationale |
|------|------|-------------|-----------|
| 1 | Read-only tools | No (allowlist) | Can't cause harm |
| 2 | In-project file edits | No (bypass) | Reviewable via git diff |
| 3 | Shell commands, external access, out-of-project writes | Yes | High blast radius |

**Key design decision: the classifier is reasoning-blind.** It only sees user messages and bare tool commands, never Claude's internal reasoning or tool outputs. This means Claude can't "talk the classifier into" approving something dangerous, and prompt injection in tool outputs can't influence decisions.

**Intent matching:** Actions are evaluated against what the user actually asked for. "Clean up branches" does not authorize force-deleting remote branches. A script written to a file then executed is evaluated as the assembled command, not as benign file writing.

**Real attacks caught:**

| Attack | What happened |
|--------|--------------|
| Credential exploration | Agent searched env variables for API tokens after auth failed |
| Data exfiltration | Created a public GitHub Gist with confidential debugging code |
| Safety bypass | Retried failed deploy with `--no-verify` flags |
| Scope escalation | Force-deleted remote branches from vague "clean up" instruction |

**Multi-agent safety:** Classifiers run at both ends of subagent handoffs. Outbound checks verify delegated tasks align with user intent. Return checks review action history before results propagate back.

**Performance (honest about limitations):**

| Metric | Value |
|--------|-------|
| False positive rate (blocks safe actions) | 0.4% |
| False negative on exfiltration | 5.7% |
| False negative on overeager actions | 17% |

The 17% miss rate means auto mode is not a replacement for careful human review on high-stakes infrastructure. It's designed for users who would otherwise use `--dangerously-skip-permissions`.

**Connections:**
- Trend 4 (human oversight scales): AI overseeing AI. The human reviews at a higher level.
- Trend 8 (security-first): Safety built into the architecture via defense in depth.
- Multi-agent systems (Trend 2): Classifiers at subagent boundaries show how oversight scales to agent teams.

## Community Resources

### CQ: Shared Agent Learning (Mozilla AI)

**What it is:** An open standard for AI agents to share and learn from collective knowledge. When one agent solves a problem, other agents can learn from that experience instead of rediscovering the same failure.

**Repository:** [github.com/mozilla-ai/cq](https://github.com/mozilla-ai/cq) (Apache 2.0)

**Architecture:**

```
Agent (Claude Code / OpenCode)
  └── Local MCP Server (FastMCP)
        ├── Local SQLite DB (~/.local/share/cq/local.db)
        │     Personal knowledge store, works offline
        │
        └── Team API (Docker, localhost:8742)
              Shared knowledge layer via REST
              Graduated tiers: local → team-shared
```

Three key design decisions:
1. **Local-first.** Works immediately with SQLite, no configuration. Team sync is optional.
2. **Knowledge tiers.** Insights graduate from local to team-shared. Not everything gets shared by default.
3. **Post-error hooks.** After a failure, the agent automatically queries the knowledge base.

**Installation in Claude Code:**
```bash
claude plugin marketplace add mozilla-ai/cq
claude plugin install cq
```

**Why this matters:** CQ addresses the agent memory gap that no open standard covers yet. It's a more structured alternative to prompt-engineered markdown memory, designed for multiple agents sharing a knowledge base. Exploratory/proof-of-concept stage (818 stars), worth watching as the ecosystem matures.

## The Pattern: Isolation + Graduated Trust

Sandbox, auto mode, and CQ look like different features, but they're three applications of the same security pattern at different layers of the agent stack:

```
LAYER               WHAT'S ISOLATED           GRADUATED TRUST
─────────────────────────────────────────────────────────────
Sandbox mode        The agent's actions        Tier 1: /sandbox only
                    (file access, network)     Tier 2: project files (with permission)
                                               Tier 3: full system (dangerously-skip)

Auto mode           The approval decision      Tier 1: read-only (always allowed)
                    (who decides if it's safe)  Tier 2: in-project edits (bypass, use git)
                                               Tier 3: high-risk ops (classifier evaluates)

CQ                  The knowledge layer        Tier 1: local SQLite (private)
                    (what agents remember)      Tier 2: team API in Docker (shared)
                                               Tier 3: (future) cross-org sharing
```

**The principle:** Start restricted, earn more access based on demonstrated safety. Never give full access by default. Each layer isolates a different thing (actions, decisions, knowledge), but the trust gradient is the same.

This pattern also appears in AI governance frameworks. The EU AI Act classifies systems by risk tier (minimal, limited, high, unacceptable) with graduated requirements. Anthropic's Responsible Scaling Policy does the same with model capabilities (ASL-1 through ASL-4). The technical implementation mirrors the policy design.
