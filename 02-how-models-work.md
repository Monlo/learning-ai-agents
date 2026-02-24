# How AI Models Are Built

_From raw data to a working model: the training pipeline, core mechanics, and performance measures._

## Key Concepts

| Concept | Definition | Source |
|---------|-----------|--------|
| LLM | Large Language Model -- AI trained on vast text data that can generate text, answer questions, summarize, etc. Claude is an LLM fine-tuned with RLHF | Glossary |
| Pretraining | Initial training phase on massive text data. Pretrained models predict the next word but aren't good at following instructions until fine-tuned | Glossary |
| Fine-tuning | Further training a pretrained model on specific data to adapt it to a domain, task, or style. Claude is already fine-tuned to be a helpful assistant | Glossary |
| Tokens | Smallest units the model processes. ~3.5 English characters per token. Input + output tokens = what you pay for on the API | Glossary |
| Context window | The model's "working memory" -- how much text it can look at when generating a response. NOT the same as training data. Larger window = can handle longer/more complex conversations | Glossary |
| Temperature | Parameter controlling randomness. Low = conservative/deterministic output. High = creative/diverse output. Even at 0, results aren't fully deterministic | Glossary |
| Latency | Time between sending a prompt and receiving output. Affected by model size, hardware, network, and prompt complexity | Glossary |
| TTFT | Time to First Token -- how fast the model starts responding. Key for interactive/real-time applications | Glossary |

## The Training Pipeline

```
Massive text data
       │
       ▼
  PRETRAINING          → Model learns to predict next word
       │                 (not yet good at instructions)
       ▼
  FINE-TUNING          → Trained on specific data to become
       │                 a helpful assistant
       ▼
  RLHF + CAI (RLAIF)  → Human feedback + AI self-critique
       │                 teach it to be Helpful, Honest, Harmless
       ▼
  Claude               → The model you interact with
```

## How Claude Processes Text

- **Tokens** are the atomic units. ~3.5 English characters per token. "Hello world" ≈ 2-3 tokens. Input + output tokens = API cost.
- **Context window** is Claude's "working memory." NOT the same as what it was trained on. Bigger window = longer conversations, more complex tasks.
- **Temperature** controls randomness. Low (0) = deterministic, conservative. High (1) = creative, diverse. Even at 0, not fully deterministic across API calls.

## Performance Metrics

- **Latency:** Total time from prompt to complete response.
- **TTFT:** How fast the model starts responding. Critical for chatbots and real-time apps.
