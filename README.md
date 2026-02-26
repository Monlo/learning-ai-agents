# Learning AI Agents

A learning journal documenting my deep dive into AI agents, Claude Code, and the ecosystem around them. This isn't a tutorial. It's structured notes from reading Anthropic's documentation, research papers, industry reports, and community resources, organized into a path that builds understanding layer by layer.

## Learning Path

```
01  Industry Landscape     What's happening + empirical evidence (Economic Index)
     │
02  How Models Work        The training pipeline (pretraining → RLHF → Claude)
     │
03  Safety & Alignment     Constitutional AI + Responsible Scaling Policy
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
| 01 | [Industry Landscape](01-industry-landscape.md) | Agentic coding, 8 trends shaping 2026, the shift from implementer to orchestrator, Anthropic Economic Index (empirical data on AI's labor market impact) |
| 02 | [How Models Work](02-how-models-work.md) | LLMs, tokens, context windows, temperature, the training pipeline |
| 03 | [Safety & Alignment](03-safety-and-alignment.md) | RLHF, Constitutional AI, RLAIF, the HHH framework, Responsible Scaling Policy (organizational governance) |
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
1. **Key concepts table** -- term, definition, linked source
2. **Diagrams** -- ASCII architecture and flow diagrams
3. **Detailed notes** -- organized by subtopic
4. **Cross-references** -- how concepts link to other files in this repo

The files build on each other (01 → 05), but each one stands alone if you want to jump to a specific topic.

## Sources

All resources are documented in [resources.md](resources.md), including:
- [2026 Agentic Coding Trends Report](https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf) (Anthropic)
- [Agent Skills Overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) (Anthropic Platform Docs)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/docs/getting-started/intro) docs
- [Claude Glossary](https://platform.claude.com/docs/en/about-claude/glossary)
- [Constitutional AI Paper](https://arxiv.org/abs/2212.08073) (Bai et al.)
- [Claude Code Best Practices](https://github.com/shanraisshan/claude-code-best-practice) (community repo)
- [Responsible Scaling Policy v2.2](https://www-cdn.anthropic.com/872c653b2d0501d6ab44cf87f43e1dc4853e4d37.pdf) (Anthropic)
- [Anthropic Economic Index](https://www.anthropic.com/economic-index#country-usage) + [Building Blocks blog post](https://www.anthropic.com/research/economic-index-primitives)

---

*Started February 2026. Updated as I learn more.*
