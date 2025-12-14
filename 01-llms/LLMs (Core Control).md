# 01-llms — LLMs (Core Control)

This module focuses on **direct control over Large Language Models (LLMs)**.  
Before building chains, tools, or agents, it is critical to understand **how LLMs are selected, configured, and invoked**.

The goal is **predictable, repeatable, and cost-aware LLM usage**.

---

## LLM Providers Covered

### ChatOpenAI
- Industry standard interface
- Used widely in production systems
- Reference implementation for most LangChain abstractions
- (Paid — not required for this repo)

### ChatGroq (FREE)
- Ultra-fast inference
- Free API tier
- Ideal for learning and experimentation
- Supports LLaMA, Mixtral-class models

### ChatOllama (LOCAL, FREE)
- Runs models locally
- No API key required
- Full control over data and privacy
- Best for offline development and testing

> This repository uses **ChatGroq** and **ChatOllama** and **GeminiAPI** to stay 100% free.

---

## Key Concepts to Understand

### Model Selection

Choosing a model affects:
- Response quality
- Latency
- Cost
- Context window size

Examples:
- Smaller models → faster, cheaper
- Larger models → better reasoning, higher cost

**Enterprise rule**:  
> Always select the *smallest model that meets the task requirements*.
## Model Selection — Core Concepts

Before selecting any LLM, it is critical to understand **what trade-offs you are making**.  
Model selection is **not about choosing the “best” model**, but the **right model for the task**.

Every model decision affects **quality, speed, cost, and limits**.

---

## 1️⃣ Response Quality

**Response quality** refers to how well a model:
- Understands the question
- Reasons through the problem
- Follows instructions
- Produces accurate, structured output

### What Affects Response Quality
- Model size (number of parameters)
- Training data diversity
- Instruction tuning quality
- Reasoning capability

### Practical Examples
- Simple Q&A → smaller models are sufficient
- Multi-step reasoning → larger models perform better
- Strict formatting (JSON, schema) → instruction-tuned models work best

### Enterprise Insight
> Higher response quality usually means **higher cost and latency**.

Do **not** use large models unless the task truly requires them.

---

## 2️⃣ Latency

**Latency** is the time taken to receive a response from the model.

It includes:
- Network time (API call)
- Model processing time
- Token generation time

### Why Latency Matters
- User experience (slow responses feel broken)
- System throughput
- Real-time applications

### Typical Patterns
- Smaller models → lower latency
- Local models → predictable latency
- Larger models → slower, especially under load

### Enterprise Rule
> If a task runs in a user-facing flow, **low latency is critical**.

High-latency models should be reserved for background or batch jobs.

---

## 3️⃣ Cost

**Cost** is determined by:
- Number of input tokens
- Number of output tokens
- Model pricing tier

Even “free” models have **hidden costs**:
- Rate limits
- Performance trade-offs
- Infrastructure overhead (for local models)

### Cost Grows With
- Larger prompts
- Long conversation history
- High max token limits
- Repeated retries

### Enterprise Rule
> Always control cost by **limiting tokens and model size**.

Cost management is part of system design, not an afterthought.

---

## 4️⃣ Context Window Size

The **context window** is the maximum number of tokens a model can process at once  
(input + output combined).

### What Uses Context Tokens
- System messages
- User messages
- Chat history
- Retrieved documents (RAG)
- Tool outputs

### Why Context Size Matters
- Small context → limited memory
- Large context → better reasoning over documents
- Oversized context → higher cost and latency

### Common Problems
- “Context overflow” errors
- Silent truncation of important data
- Increased hallucinations with too much context

### Enterprise Rule
> Bigger context is useful — but only when managed carefully.

Always:
- Chunk documents
- Retrieve selectively
- Avoid dumping raw data into prompts

---

## How These Factors Trade Off

| Factor | Improves | Usually Increases |
|-----|-------|----------------|
| Larger Model | Response Quality | Cost, Latency |
| Lower Temperature | Reliability | Determinism |
| Larger Context Window | Long reasoning | Cost, Latency |
| Smaller Model | Speed | Lower reasoning depth |

There is **no free optimization** — every improvement has a cost.

---

## How to Think Before Selecting a Model

Before choosing a model, ask:

1. Does this task require deep reasoning?
2. Is the response user-facing or background?
3. How fast must the response be?
4. How much data must the model see?
5. What is the acceptable cost per request?

### Simple Guideline
> Use the **sm**

---

### Temperature

Controls randomness in responses.

| Value | Behavior |
|-----|---------|
| `0.0` | Deterministic, factual |
| `0.2 – 0.5` | Balanced |
| `0.7+` | Creative, less predictable |

**Production default**: `0.0 – 0.3`  
High temperature increases variance and risk.

---

### Max Tokens

Defines the **maximum length of the model’s output**.

Used to:
- Control cost
- Prevent runaway responses
- Enforce response limits

**Best practice**:
- Set explicit limits
- Never rely on defaults

---

### System vs User Messages

LLMs interpret messages based on **role**.

#### System Message
- Defines behavior and constraints
- Sets tone, rules, and identity
- Has the highest priority

Example purposes:
- “You are a compliance assistant”
- “Follow company policy strictly”
- “Respond in JSON only”

#### User Message
- Represents end-user input
- Asks questions or provides instructions
- Lower priority than system messages

**Key rule**:
> System messages control behavior, user messages request actions.

---

## Why This Module Matters

Most production issues come from:
- Poor model selection
- Uncontrolled temperature
- Missing token limits
- Weak system instructions

This module ensures:
- Deterministic behavior
- Reproducible outputs
- Safe defaults for enterprise systems

---
