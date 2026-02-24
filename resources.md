# Sources & Resources

_Full summaries of every resource reviewed, with links and dates._

## Completed

| # | Resource | Type | Date | Key takeaway |
|---|----------|------|------|-------------|
| 1 | [2026 Agentic Coding Trends Report](https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf) | PDF Report (18p) | 2026-02-23 | Software development shifts from writing code to orchestrating agents; humans become directors, not implementers |
| 2 | [Agent Skills Overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) | Docs | 2026-02-23 | Skills are the mechanism for turning general-purpose Claude into a domain specialist -- loaded on demand via progressive disclosure |
| 3 | [MCP Intro](https://modelcontextprotocol.io/docs/getting-started/intro) + [Architecture](https://modelcontextprotocol.io/docs/learn/architecture) + [Server Concepts](https://modelcontextprotocol.io/docs/learn/server-concepts) + [Claude Code MCP](https://code.claude.com/docs/en/mcp) | Docs (4 pages) | 2026-02-23 | MCP is the universal protocol for AI-to-world connections; Skills = what to do, MCP = access to do it |
| 4 | [Claude Glossary](https://platform.claude.com/docs/en/about-claude/glossary) | Docs | 2026-02-23 | Core vocabulary: training pipeline (pretraining → fine-tuning → RLHF), operational concepts (tokens, context window, temperature), connection layer (MCP, RAG) |
| 5 | [Constitutional AI Paper](https://www-cdn.anthropic.com/7512771452629584566b6303311496c262da1006/Anthropic_ConstitutionalAI_v2.pdf) ([full paper](https://arxiv.org/abs/2212.08073)) | Research paper | 2026-02-23 | Give the model constitutional principles for self-critique (RLAIF); achieves Pareto improvement -- more helpful AND more harmless simultaneously |
| 6 | [Claude Code Best Practices](https://github.com/shanraisshan/claude-code-best-practice) (4k+ stars) | GitHub repo | 2026-02-23 | Community-curated patterns: Command→Agent→Skill architecture, Boris Cherny's 12 tips, hooks, rules, sandbox, MCP server recommendations, context engineering best practices |

## Detailed Summaries

<details>
<summary><strong>1. 2026 Agentic Coding Trends Report</strong> (Anthropic)</summary>

18-page industry report covering 8 trends across three categories: Foundation (how development work changes), Capability (what agents can do), and Impact (what agents change in business). Key insight: engineers shift from "implementer" to "orchestrator." The collaboration paradox -- AI is used in ~60% of work but only 0-20% can be fully delegated. Four priorities for 2026: multi-agent coordination, human-agent oversight, expanding beyond engineering, security-first architecture.

→ See [01-industry-landscape.md](01-industry-landscape.md)
</details>

<details>
<summary><strong>2. Agent Skills Overview</strong> (Anthropic Platform Docs)</summary>

Official documentation for Agent Skills -- modular, filesystem-based capability packages. Three-level progressive disclosure (metadata → instructions → resources) minimizes token cost. Skills work across Claude.ai, the API, Claude Code, and the Agent SDK. Pre-built skills for Office suite; custom skills via SKILL.md with YAML frontmatter. Security model: treat skills like installed software.

→ See [05-practical-claude-code.md](05-practical-claude-code.md)
</details>

<details>
<summary><strong>3. Model Context Protocol (MCP)</strong> (4 documentation pages)</summary>

Open-source standard for connecting AI to external systems. Architecture: Host → Client → Server. Three primitives: Tools (model-controlled actions), Resources (application-controlled data), Prompts (user-controlled templates). Two transport layers: STDIO (local) and Streamable HTTP (remote with OAuth). JSON-RPC 2.0 for communication. Lifecycle: initialize → discover → execute → notify → shutdown.

→ See [04-connecting-to-the-world.md](04-connecting-to-the-world.md)
</details>

<details>
<summary><strong>4. Claude Glossary</strong> (Anthropic Platform Docs)</summary>

Core vocabulary covering three areas: (1) Training pipeline -- pretraining, fine-tuning, RLHF; (2) Operational concepts -- tokens (~3.5 chars each), context window (working memory), temperature (randomness), latency, TTFT; (3) Connection layer -- MCP, RAG, MCP Connector. Essential foundation for understanding how models are built and how they process text.

→ See [02-how-models-work.md](02-how-models-work.md)
</details>

<details>
<summary><strong>5. Constitutional AI Paper</strong> (Bai et al., Anthropic)</summary>

Research paper introducing Constitutional AI (CAI) -- an alternative to pure RLHF. The core problem: standard RLHF creates a helpfulness-vs-harmlessness tradeoff (crowdworkers reward evasiveness). CAI solution: give the model written principles ("a constitution") for self-critique and self-revision. RLAIF (RL from AI Feedback) replaces/supplements human feedback. Result: Pareto improvement -- simultaneously more helpful AND more harmless. Policy implications: transparent principles readable by regulators, lower alignment costs, but dual-use risk.

→ See [03-safety-and-alignment.md](03-safety-and-alignment.md)
</details>

<details>
<summary><strong>6. Claude Code Best Practices</strong> (Community GitHub repo, 4k+ stars)</summary>

Community-curated collection of patterns and tips. Key contributions: Command→Agent→Skill architecture pattern, Boris Cherny's 12 tips, hooks system, rules with path-scoping, sandbox mode, MCP server recommendations (Context7, Playwright, Chrome, DeepWiki, Excalidraw), advanced API features (Programmatic Tool Calling, Tool Search, Dynamic Filtering), and workflow best practices (CLAUDE.md under 150 lines, /compact at 50% context, plan mode for complex tasks).

→ See [05-practical-claude-code.md](05-practical-claude-code.md)
</details>

## To Review

| Resource | Type | Status |
|----------|------|--------|
| [Anthropic Economic Index](https://www.anthropic.com/economic-index#country-usage) | Report | Queued |
| [AI-orchestrated cyber espionage report](https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf) | PDF | Queued |
