# How Models Learn Right from Wrong

_Safety and alignment: from human feedback to constitutional principles to organizational governance._

**Key sources:** [Constitutional AI Paper](https://arxiv.org/abs/2212.08073) (Bai et al., Anthropic) · [Responsible Scaling Policy v2.2](https://www-cdn.anthropic.com/872c653b2d0501d6ab44cf87f43e1dc4853e4d37.pdf) · [Claude Glossary](https://platform.claude.com/docs/en/about-claude/glossary)

---

## Key Concepts

| Concept | Definition | Source |
|---------|-----------|--------|
| RLHF | Reinforcement Learning from Human Feedback -- how Claude was trained to align with human preferences (helpful, honest, harmless) | [Glossary](https://platform.claude.com/docs/en/about-claude/glossary) |
| HHH | Anthropic's core goals: **Helpful** (performs tasks well), **Honest** (accurate, acknowledges limits), **Harmless** (refuses dangerous/unethical requests) | [Glossary](https://platform.claude.com/docs/en/about-claude/glossary) |
| Constitutional AI (CAI) | Anthropic's approach to AI safety: give the model a set of principles ("constitution") so it can evaluate and revise its own outputs. Reduces the helpfulness-vs-harmlessness tradeoff | [CAI Paper](https://arxiv.org/abs/2212.08073) |
| RLAIF | Reinforcement Learning from AI Feedback -- the model critiques itself using constitutional principles instead of relying entirely on human crowdworkers | [CAI Paper](https://arxiv.org/abs/2212.08073) |
| Helpfulness vs harmlessness tradeoff | Standard RLHF makes models either helpful OR harmless. A model that always says "I can't answer that" is harmless but useless. CAI achieves both simultaneously (Pareto improvement) | [CAI Paper](https://arxiv.org/abs/2212.08073) |
| Responsible Scaling Policy (RSP) | Anthropic's governance framework for safely scaling AI: mandatory evaluations, red-teaming, and hard stop commitments tied to capability tiers | [RSP v2.2](https://www-cdn.anthropic.com/872c653b2d0501d6ab44cf87f43e1dc4853e4d37.pdf) |

---

## Two Layers of Safety

This section covers two complementary mechanisms. Understanding both — and how they connect — is key to seeing how AI safety works in practice, not just in theory.

```
LAYER 1: MODEL-LEVEL                    LAYER 2: ORGANIZATIONAL-LEVEL
Constitutional AI (CAI)                  Responsible Scaling Policy (RSP)
        │                                        │
        ▼                                        ▼
How the model learns                     When it's safe to release
values during training                   more powerful models
        │                                        │
        ▼                                        ▼
Self-critique + revision                 Mandatory evaluations,
using written principles                 red-teaming, hard stops
        │                                        │
        └────────────── together ────────────────┘
                         │
                         ▼
              Safety that scales with capability
```

---

## Layer 1: Constitutional AI (CAI)

### The Problem

Standard RLHF has a fundamental tension:

```
HARMLESS  ◄─────────── tradeoff ───────────►  HELPFUL
   │                                              │
   ▼                                              ▼
"I can't answer that"                   Answers everything
(safe but useless)                      (useful but risky)
```

Human crowdworkers tend to **reward evasiveness** -- they prefer a model that refuses over one that takes risks. This makes models safe but frustratingly unhelpful.

This tension also shows up empirically: the [Economic Index](01-industry-landscape.md#measuring-the-impact-anthropic-economic-index) finds AI use is currently 52% augmentation vs. 45% automation — getting the balance right between doing too much and too little is a recurring theme.

### How CAI Works

Instead of relying entirely on human judges, [CAI](https://arxiv.org/abs/2212.08073) gives the model a **set of principles** (a "constitution") and lets it evaluate its own outputs:

1. **Self-critique.** The model generates a response, then critiques it against constitutional principles.
2. **Self-revision.** The model revises based on the critique.
3. **RLAIF.** Reinforcement Learning from **AI** Feedback -- self-critiques create training data, replacing/supplementing human crowdworkers.

**Example principle:** "Which of these assistant responses is less harmful? Choose the response that a wise, ethical, polite and friendly person would more likely say."

### Why CAI Matters

1. **Pareto improvement:** Constitutional RL models are simultaneously MORE helpful AND MORE harmless than standard RLHF. Win-win, not tradeoff.
2. **Transparency:** Principles are written in natural language -- you can READ what the model is optimizing for. Legible to users, regulators, auditors.
3. **Scalability:** Much less resource-intensive than collecting tens of thousands of human feedback labels. Doesn't require exposing crowdworkers to offensive outputs.

### Policy Implications of CAI

- **Democratic input:** Drafting a constitution for AI could involve diverse stakeholders -- community, cultural, organizational input.
- **Lower alignment costs:** Incentivizes more developers to make models safe.
- **Dual-use risk:** CAI could also make it easier to train harmful systems (echoes [Trend 8](01-industry-landscape.md#impact--what-agents-change-in-business)).
- **Not a replacement for testing:** Helps but doesn't eliminate the need for robust evaluation — which is exactly what Layer 2 addresses.

---

## Layer 2: Responsible Scaling Policy (RSP)

_CAI teaches the model right from wrong. The [RSP](https://www-cdn.anthropic.com/872c653b2d0501d6ab44cf87f43e1dc4853e4d37.pdf) governs when it's safe to make models more powerful._

Anthropic's RSP v2.2 is a concrete governance framework — not abstract principles, but specific rules for when to proceed or pause AI development.

### Four Risk Categories

| Category | What it covers |
|----------|---------------|
| **High-risk capabilities** | Persuasion, code generation, advanced reasoning |
| **Misuse potential** | Applications that could enable harm |
| **Model deception & autonomy** | Systems behaving unexpectedly or pursuing goals independently |
| **Systemic risks** | Broader societal impacts beyond individual use cases |

### Staged Deployment

Each capability tier requires specific safety evaluations before proceeding to the next:

1. **Mandatory safety evaluations** before deploying each new model version
2. **Red-teaming and adversarial testing** to identify failure modes
3. **Capability assessments** for high-risk functions (coding, planning, reasoning)
4. **Post-deployment monitoring** to detect misuse
5. **External consultation** with domain experts on emerging risks
6. **Hard stop commitment** — development pauses if safety thresholds aren't met

### Key Insight

Higher-capability models face stricter evaluation criteria. Specific high-risk capabilities (persuasion, deception potential) get dedicated testing frameworks. This is the practical answer to the [dual-use risk from Trend 8](01-industry-landscape.md#impact--what-agents-change-in-business) — you don't just hope models are safe, you build governance that *scales with capability*.

---

## How the Two Layers Connect

| | CAI (Layer 1) | RSP (Layer 2) |
|---|---|---|
| **Scope** | Training mechanism | Organizational mechanism |
| **Question it answers** | How does the model learn values? | Should we release this model? |
| **Method** | Self-critique against written principles | Mandatory evaluations + hard stops |
| **When it applies** | During model development | Before and after deployment |
| **Who's involved** | AI researchers + the model itself | Red teams, domain experts, leadership |

Together they form a safety architecture where the model is trained to be safe (CAI) **and** the organization verifies it actually is before releasing it (RSP). Neither alone is sufficient — a well-trained model still needs external validation, and evaluations are only meaningful if the model was trained with safety in mind.

This connects directly to [how Claude is built](02-how-models-work.md#the-training-pipeline): pretraining → fine-tuning → RLHF + CAI → then RSP evaluation before release.
