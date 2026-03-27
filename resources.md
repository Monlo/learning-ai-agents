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
| 7 | [Responsible Scaling Policy v2.2](https://www-cdn.anthropic.com/872c653b2d0501d6ab44cf87f43e1dc4853e4d37.pdf) | Policy document | 2026-02-24 | Staged deployment governance: 4 risk categories, mandatory red-teaming, hard stop if safety thresholds aren't met. CAI = training-level safety; RSP = organizational-level safety |
| 8 | [Anthropic Economic Index](https://www.anthropic.com/economic-index#country-usage) | Interactive dashboard | 2026-02-26 | Dashboard exploring Claude usage across US states, countries, and hundreds of occupations; tracks augmentation vs. automation and trending topics by region |
| 9 | [Economic Index: New Building Blocks](https://www.anthropic.com/research/economic-index-primitives) | Research blog post | 2026-02-26 | 5 economic primitives for measuring AI impact; 12x speedup on college-level tasks; 49% of jobs exposed; deskilling risk as AI handles higher-skill tasks; 1.0-1.2pp productivity growth |
| 10 | [CQ: Open Standard for Shared Agent Learning](https://github.com/mozilla-ai/cq) | GitHub repo (Mozilla AI) | 2026-03-27 | Local-first knowledge persistence for AI agents; graduated sharing tiers; post-error hooks for collective learning; Claude Code plugin |
| 11 | [Claude Code auto mode](https://www.anthropic.com/engineering/claude-code-auto-mode) | Engineering blog (Anthropic) | 2026-03-27 | Two-stage AI classifier replaces manual permission prompts; reasoning-blind design prevents prompt injection; 0.4% FPR but 17% miss rate on overeager actions |

## Detailed Summaries

<details>
<summary><strong>1. 2026 Agentic Coding Trends Report</strong> (Anthropic)</summary>

18-page industry report covering 8 trends across three categories: Foundation (how development work changes), Capability (what agents can do), and Impact (what agents change in business). Key insight: engineers shift from "implementer" to "orchestrator." The collaboration paradox -- AI is used in ~60% of work but only 0-20% can be fully delegated. Four priorities for 2026: multi-agent coordination, human-agent oversight, expanding beyond engineering, security-first architecture.

> See [01-industry-landscape.md](01-industry-landscape.md)
</details>

<details>
<summary><strong>2. Agent Skills Overview</strong> (Anthropic Platform Docs)</summary>

Official documentation for Agent Skills -- modular, filesystem-based capability packages. Three-level progressive disclosure (metadata → instructions → resources) minimizes token cost. Skills work across Claude.ai, the API, Claude Code, and the Agent SDK. Pre-built skills for Office suite; custom skills via SKILL.md with YAML frontmatter. Security model: treat skills like installed software.

> See [05-practical-claude-code.md](05-practical-claude-code.md)
</details>

<details>
<summary><strong>3. Model Context Protocol (MCP)</strong> (4 documentation pages)</summary>

Open-source standard for connecting AI to external systems. Architecture: Host → Client → Server. Three primitives: Tools (model-controlled actions), Resources (application-controlled data), Prompts (user-controlled templates). Two transport layers: STDIO (local) and Streamable HTTP (remote with OAuth). JSON-RPC 2.0 for communication. Lifecycle: initialize → discover → execute → notify → shutdown.

> See [04-connecting-to-the-world.md](04-connecting-to-the-world.md)
</details>

<details>
<summary><strong>4. Claude Glossary</strong> (Anthropic Platform Docs)</summary>

Core vocabulary covering three areas: (1) Training pipeline -- pretraining, fine-tuning, RLHF; (2) Operational concepts -- tokens (~3.5 chars each), context window (working memory), temperature (randomness), latency, TTFT; (3) Connection layer -- MCP, RAG, MCP Connector. Essential foundation for understanding how models are built and how they process text.

> See [02-how-models-work.md](02-how-models-work.md)
</details>

<details>
<summary><strong>5. Constitutional AI Paper</strong> (Bai et al., Anthropic)</summary>

Research paper introducing Constitutional AI (CAI) -- an alternative to pure RLHF. The core problem: standard RLHF creates a helpfulness-vs-harmlessness tradeoff (crowdworkers reward evasiveness). CAI solution: give the model written principles ("a constitution") for self-critique and self-revision. RLAIF (RL from AI Feedback) replaces/supplements human feedback. Result: Pareto improvement -- simultaneously more helpful AND more harmless. Policy implications: transparent principles readable by regulators, lower alignment costs, but dual-use risk.

> See [03-safety-and-alignment.md](03-safety-and-alignment.md)
</details>

<details>
<summary><strong>6. Claude Code Best Practices</strong> (Community GitHub repo, 4k+ stars)</summary>

Community-curated collection of patterns and tips. Key contributions: Command→Agent→Skill architecture pattern, Boris Cherny's 12 tips, hooks system, rules with path-scoping, sandbox mode, MCP server recommendations (Context7, Playwright, Chrome, DeepWiki, Excalidraw), advanced API features (Programmatic Tool Calling, Tool Search, Dynamic Filtering), and workflow best practices (CLAUDE.md under 150 lines, /compact at 50% context, plan mode for complex tasks).

> See [05-practical-claude-code.md](05-practical-claude-code.md)
</details>

<details>
<summary><strong>7. Responsible Scaling Policy v2.2</strong> (Anthropic)</summary>

Anthropic's governance framework for safely scaling AI — concrete rules, not abstract principles. Defines four risk categories (high-risk capabilities, misuse potential, model deception & autonomy, systemic risks) and requires staged evaluations before deploying each capability tier. Includes mandatory red-teaming, capability assessments for high-risk functions, post-deployment monitoring, external consultation, and a hard stop commitment if safety thresholds aren't met. Key insight: higher-capability models face stricter evaluation. Connects to CAI as two complementary safety layers — model-level (training) and organizational-level (deployment).

> See [03-safety-and-alignment.md](03-safety-and-alignment.md#layer-2-responsible-scaling-policy-rsp)
</details>

<details>
<summary><strong>8. Anthropic Economic Index</strong> (Interactive Dashboard)</summary>

Interactive dashboard visualizing how Claude is used across the economy. Explore data by US state, country, occupation, and task type. Tracks augmentation vs. automation balance, trending topics by region, and occupational exposure over time. Companion to the research blog post (#9) — the dashboard provides the exploratory interface while the blog post provides the analytical framework.

> See [01-industry-landscape.md](01-industry-landscape.md#measuring-the-impact-anthropic-economic-index)
</details>

<details>
<summary><strong>9. Economic Index: New Building Blocks for Understanding AI Use</strong> (Anthropic Research)</summary>

Research blog post introducing 5 economic primitives for systematically measuring AI's labor market impact: task complexity, human & AI skills, use case, AI autonomy, and task success. Based on analysis of millions of Claude conversations (1M consumer + 1M API). Key findings: 12x speedup on college-level tasks, 49% of jobs have significant Claude exposure, deskilling risk as AI preferentially handles higher-skill tasks, currently 52% augmentation vs. 45% automation, and estimated 1.0-1.2 percentage points/year productivity growth. Geographic patterns show richer countries favor work/personal use while lower-income countries emphasize educational use.

> See [01-industry-landscape.md](01-industry-landscape.md#measuring-the-impact-anthropic-economic-index)
</details>

<details>
<summary><strong>10. CQ: Shared Agent Learning</strong> (Mozilla AI)</summary>

Open-source framework (Apache 2.0) enabling AI agents to share and learn from collective knowledge. Local-first design with SQLite, optional team sync via REST API. Three key features: graduated knowledge tiers (local → team-shared), post-error hooks that automatically query the knowledge base after failures, and multi-runtime support (Claude Code plugin + OpenCode MCP server). Addresses the agent memory gap -- no open standard exists for how agents persist and share knowledge across sessions.

> See [05-practical-claude-code.md](05-practical-claude-code.md#community-resources) and [04-connecting-to-the-world.md](04-connecting-to-the-world.md#open-standards-for-ai-agents)
</details>

<details>
<summary><strong>11. Claude Code auto mode</strong> (Anthropic Engineering)</summary>

Engineering blog post explaining how auto mode replaces manual permission prompts with AI classifiers. Two-stage architecture: fast single-token filter (Stage 1) plus chain-of-thought reasoning for flagged actions (Stage 2). Three trust tiers: read-only (always allowed), in-project edits (bypass, reviewable via git), high-risk operations (classifier evaluates). Key design decision: classifier is reasoning-blind -- it never sees Claude's internal reasoning or tool outputs, preventing prompt injection and self-justification. Performance: 0.4% false positive rate, 5.7% miss rate on exfiltration, 17% miss rate on overeager actions. Multi-agent safety via classifiers at subagent boundaries.

> See [05-practical-claude-code.md](05-practical-claude-code.md#auto-mode-ai-classifiers-as-safety-layer)
</details>

## To Review

| Resource                                                                                                                                                                                            | Type            | Status |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- | ------ |
| [Economic Index Report (PDF)](https://www-cdn.anthropic.com/c788cbc0a3da9135112f97cdf6dcd06f2c16cee2.pdf)                                                                                           | PDF report      | Queued |
| [Sabotage Risk Report: Claude Opus 4.6](https://www-cdn.anthropic.com/f21d93f21602ead5cdbecb8c8e1c765759d9e232.pdf)                                                                                 | Research report | Queued |
| [Disrupting the First AI-Orchestrated Cyber Espionage Campaign](https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf) | Report          | Queued |
| https://www.youtube.com/watch?v=GDm_uH6VxPY                                                                                                                                                         |                 |        |
