# How Models Learn Right from Wrong

_Safety and alignment: from human feedback to constitutional principles._

## Key Concepts

| Concept | Definition | Source |
|---------|-----------|--------|
| RLHF | Reinforcement Learning from Human Feedback -- how Claude was trained to align with human preferences (helpful, honest, harmless) | Glossary |
| HHH | Anthropic's core goals: **Helpful** (performs tasks well), **Honest** (accurate, acknowledges limits), **Harmless** (refuses dangerous/unethical requests) | Glossary |
| Constitutional AI (CAI) | Anthropic's approach to AI safety: give the model a set of principles ("constitution") so it can evaluate and revise its own outputs. Reduces the helpfulness-vs-harmlessness tradeoff | CAI Paper |
| RLAIF | Reinforcement Learning from AI Feedback -- the model critiques itself using constitutional principles instead of relying entirely on human crowdworkers | CAI Paper |
| Helpfulness vs harmlessness tradeoff | Standard RLHF makes models either helpful OR harmless. A model that always says "I can't answer that" is harmless but useless. CAI achieves both simultaneously (Pareto improvement) | CAI Paper |

## The Problem CAI Solves

Standard RLHF has a fundamental tension:

```
HARMLESS  ◄─────────── tradeoff ───────────►  HELPFUL
   │                                              │
   ▼                                              ▼
"I can't answer that"                   Answers everything
(safe but useless)                      (useful but risky)
```

Human crowdworkers tend to **reward evasiveness** -- they prefer a model that refuses over one that takes risks. This makes models safe but frustratingly unhelpful.

## How CAI Works

Instead of relying entirely on human judges, CAI gives the model a **set of principles** (a "constitution") and lets it evaluate its own outputs:

1. **Self-critique.** The model generates a response, then critiques it against constitutional principles.
2. **Self-revision.** The model revises based on the critique.
3. **RLAIF.** Reinforcement Learning from **AI** Feedback -- self-critiques create training data, replacing/supplementing human crowdworkers.

**Example principle:** "Which of these assistant responses is less harmful? Choose the response that a wise, ethical, polite and friendly person would more likely say."

## Why CAI Matters

1. **Pareto improvement:** Constitutional RL models are simultaneously MORE helpful AND MORE harmless than standard RLHF. Win-win, not tradeoff.
2. **Transparency:** Principles are written in natural language -- you can READ what the model is optimizing for. Legible to users, regulators, auditors.
3. **Scalability:** Much less resource-intensive than collecting tens of thousands of human feedback labels. Doesn't require exposing crowdworkers to offensive outputs.

## Policy Implications

- **Democratic input:** Drafting a constitution for AI could involve diverse stakeholders -- community, cultural, organizational input.
- **Lower alignment costs:** Incentivizes more developers to make models safe.
- **Dual-use risk:** CAI could also make it easier to train harmful systems (echoes Trend 8).
- **Not a replacement for testing:** Helps but doesn't eliminate the need for robust evaluation.
