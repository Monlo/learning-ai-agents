# Learning AI Agents

A learning journal documenting my deep dive into AI agents, Claude Code, and the ecosystem around them . This isn't a tutorial. It's structured notes from reading Anthropic's documentation, research papers, industry reports, and community resources, organized into a path that builds understanding layer by layer.

## Learning Path

```
01  Industry Landscape     What's happening in the field
     │
02  How Models Work        The training pipeline (pretraining → RLHF → Claude)
     │
03  Safety & Alignment     Constitutional AI and the helpfulness-harmlessness tradeoff
     │
04  Connecting to the World   MCP, RAG, and how models reach external tools
     │
05  Practical Claude Code  Skills, agents, hooks, MCP servers, daily workflow
     │
     └── Resources         Every source reviewed, with detailed summaries
```

## Files

| # | File | What it covers |
|---|------|---------------|
| 01 | [Industry Landscape](01-industry-landscape.md) | Agentic coding, 8 trends shaping 2026, the shift from implementer to orchestrator |
| 02 | [How Models Work](02-how-models-work.md) | LLMs, tokens, context windows, temperature, the training pipeline |
| 03 | [Safety & Alignment](03-safety-and-alignment.md) | RLHF, Constitutional AI, RLAIF, the HHH framework |
| 04 | [Connecting to the World](04-connecting-to-the-world.md) | MCP architecture, three primitives (tools/resources/prompts), RAG |
| 05 | [Practical Claude Code](05-practical-claude-code.md) | Agent Skills, custom agents, hooks, MCP servers, best practices |
| -- | [Resources](resources.md) | All sources with links, dates, and collapsible detailed summaries |

## What I'm Exploring

- [x] Basic prompting and conversation
- [x] File reading and editing
- [ ] Code generation and refactoring
- [x] Using subagents and task delegation
- [ ] MCP servers and integrations
- [ ] Hooks and customization
- [x] CLAUDE.md project memory
- [x] Agent Skills (custom skills built and working)
- [ ] Custom Agents (`.claude/agents/`)
- [ ] Rules (`.claude/rules/`)
- [ ] Sandbox mode
- [ ] Output styles (Explanatory, Learning, Custom)
- [ ] Plugins

## How this repo is organized

Each file follows the same structure:
1. **Key concepts table** -- term, definition, source
2. **Diagrams** -- ASCII architecture and flow diagrams
3. **Detailed notes** -- organized by subtopic
4. **Connections** -- how concepts link to other files

The files build on each other (01 → 05), but each one stands alone if you want to jump to a specific topic.

## Sources

All resources are documented in [resources.md](resources.md), including:
- Anthropic's 2026 Agentic Coding Trends Report
- Agent Skills documentation
- Model Context Protocol (MCP) docs
- Claude Glossary
- Constitutional AI research paper
- Claude Code Best Practices (community repo)

---

*Started February 2026. Updated as I learn more.*
