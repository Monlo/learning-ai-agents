# Learning AI Agents

A learning journal and experiment lab for AI agents, Claude Code, and the ecosystem around them. Not a tutorial. Structured notes from reading documentation, research papers, and community resources, combined with hands-on experiments that test what I've learned.

## How this repo is organized

Three layers: **topics** (concepts), **experiments** (practice), and **resources** (sources).

```
topics/                  Organized knowledge by theme
  01 Industry Landscape     What's happening + empirical evidence
  02 How Models Work        Training pipeline (pretraining → RLHF → Claude)
  03 Safety & Alignment     CAI + RSP + Sabotage Risk Assessment
  04 Connecting to World    MCP, RAG, open standards, agent memory
  05 Practical Claude Code  Skills, agents, hooks, auto mode, community tools

experiments/             Hands-on builds tied to topics
  Each experiment: README + runnable code + LEARNINGS.md

resources.md             Every source reviewed, with detailed summaries
TOPIC_MAP.md             Keyword routing for organizing new sources
```

## Topics

| # | Topic | What it covers |
|---|-------|---------------|
| 01 | [Industry Landscape](topics/01-industry-landscape.md) | Agentic coding, 8 trends shaping 2026, the shift from implementer to orchestrator, Anthropic Economic Index |
| 02 | [How Models Work](topics/02-how-models-work.md) | LLMs, tokens, context windows, temperature, the training pipeline |
| 03 | [Safety & Alignment](topics/03-safety-and-alignment.md) | Constitutional AI, RLAIF, HHH framework, Responsible Scaling Policy, Sabotage Risk Assessment (3 layers) |
| 04 | [Connecting to the World](topics/04-connecting-to-the-world.md) | MCP architecture, three primitives, RAG, open standards landscape, agent memory |
| 05 | [Practical Claude Code](topics/05-practical-claude-code.md) | Agent Skills, custom agents, hooks, MCP servers, auto mode, CQ, isolation pattern, best practices |

## Experiments

| # | Experiment | Topic | Difficulty | Status |
|---|-----------|-------|-----------|--------|
| 1 | [Custom hook](experiments/) | 05 | Beginner | Planned |
| 2 | [MCP hello-world server](experiments/) | 04 | Intermediate | Planned |
| 3 | [Build a skill from scratch](experiments/) | 05 | Beginner | Planned |

See [experiments/README.md](experiments/README.md) for the full index.

## What I'm Exploring

- [x] Basic prompting and conversation
- [x] File reading and editing
- [ ] Code generation and refactoring
- [x] Using subagents and task delegation
- [x] MCP servers and integrations (Canva MCP server in use)
- [x] Hooks and customization (post-error hooks, auto memory)
- [x] CLAUDE.md project memory
- [x] Agent Skills (custom skills built and working)
- [x] Custom Agents (`.claude/agents/`)
- [x] Rules (`.claude/rules/` -- path-scoped rules)
- [ ] Sandbox mode
- [x] Output styles (Learning mode active)
- [ ] Plugins

## Sources

12 sources reviewed. Full list with detailed summaries in [resources.md](resources.md).

Key sources:
- [2026 Agentic Coding Trends Report](https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf) (Anthropic)
- [Constitutional AI Paper](https://arxiv.org/abs/2212.08073) (Bai et al.)
- [Responsible Scaling Policy v2.2](https://www-cdn.anthropic.com/872c653b2d0501d6ab44cf87f43e1dc4853e4d37.pdf) (Anthropic)
- [Sabotage Risk Report: Claude Opus 4.6](https://www-cdn.anthropic.com/f21d93f21602ead5cdbecb8c8e1c765759d9e232.pdf) (Anthropic)
- [Claude Code auto mode](https://www.anthropic.com/engineering/claude-code-auto-mode) (Anthropic)
- [CQ: Shared Agent Learning](https://github.com/mozilla-ai/cq) (Mozilla AI)
- [Anthropic Economic Index](https://www.anthropic.com/economic-index) + [Building Blocks](https://www.anthropic.com/research/economic-index-primitives)

---

*Started February 2026. Updated as I learn more.*
