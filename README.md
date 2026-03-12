# AI & Automation Projects

This repository is dedicated to exploring Artificial Intelligence, Generative AI (Gen AI), and workflow automation using tools like **n8n**. It serves as a central hub for projects, experiments, and knowledge bases.

---

## 🤖 Basic AI Concepts & Terms

- **Artificial Intelligence (AI):** The simulation of human intelligence processes by machines, especially computer systems. This includes learning, reasoning, and self-correction.
- **Machine Learning (ML):** A subset of AI that allows systems to automatically learn and improve from experience without being explicitly programmed.
- **Deep Learning (DL):** A specialized subset of Machine Learning based on artificial neural networks with multiple layers (hence "deep"). It's the technology behind self-driving cars and voice assistants.
- **Natural Language Processing (NLP):** A branch of AI focused on the interaction between computers and human language, allowing machines to read, understand, and derive meaning from text or speech.
- **Large Language Model (LLM):** A type of AI model (like Google Gemini, OpenAI's GPT-4, or Anthropic's Claude) trained on massive amounts of text data to understand and generate human-like language.
- **Prompt Engineering:** The skill or practice of designing, structuring, and refining text inputs (prompts) to get optimal, accurate, or creative outputs from Generative AI models.

---

## 🧠 Context, Context Windows & Context Engineering

### What is Context?

In the world of LLMs, **context** is everything the model can "see" and reason over at one time. This includes:

- The **system prompt** (instructions defining the model's role and behavior)
- The **conversation history** (all prior user and assistant turns)
- Any **documents, code, or files** you paste or attach
- **Tool call inputs and outputs** (what tools were called and what they returned)
- **Retrieved data** from RAG pipelines or memory systems

The model has no memory between sessions — it only knows what is inside the current context. Everything outside it is invisible.

### What is a Context Window?

The **context window** is the maximum amount of text (measured in tokens) a model can process in a single request — both input and output combined.

| Model | Context Window |
|-------|---------------|
| GPT-4o | 128K tokens |
| Claude Sonnet 4.6 | 200K tokens |
| Claude Opus 4.6 | 200K tokens |
| Gemini 1.5 Pro | 1M tokens |
| Gemini 1.5 Flash | 1M tokens |
| Gemini 2.0 Pro Experimental | 2M tokens |

**Rough size guide:**
- 1K tokens ≈ 750 words ≈ ~3 pages of text
- 100K tokens ≈ a short novel (~75,000 words)
- 200K tokens ≈ the entire codebase of a mid-size app
- 1M tokens ≈ ~750,000 words — multiple full books or a very large repository

### The 1M+ Token Window — What It Unlocks

Models like Gemini 1.5 Pro/Flash with **1 million token context windows** fundamentally change what's possible:

- **Entire codebase in context** — load all source files of a large project at once; ask questions about any part, refactor across files, trace bugs end-to-end.
- **Full document analysis** — ingest hundreds of PDFs, contracts, or research papers in a single call and reason across all of them simultaneously.
- **Long conversation memory** — keep hundreds of turns without needing external memory systems.
- **Large data processing** — analyze entire CSV files, logs, or database dumps without chunking.
- **Many-shot prompting** — include dozens or hundreds of examples in the prompt for high-accuracy pattern learning.
- **Full audio/video transcripts** — process multi-hour recordings as a single context.

**Trade-offs of large windows:**
- Higher latency — more tokens = slower time-to-first-token
- Higher cost — pricing is per token (input + output)
- **Lost-in-the-middle problem** — LLMs tend to recall content from the beginning and end of context better than the middle; placing critical instructions mid-context can degrade performance
- Not a substitute for good prompt structure — a 1M window with a poorly organized prompt still produces worse results than a tight 10K window with crisp, relevant context

### Context Engineering

**Context engineering** is the discipline of designing, curating, and managing what goes into a model's context window to maximize output quality, cost efficiency, and reliability.

It sits above prompt engineering — while prompt engineering focuses on how you phrase instructions, context engineering decides *what information* to include, *how much*, *in what order*, and *when to compress or discard*.

#### Core Principles

**1. Relevance over volume**
Only include information that is actually needed for the current task. Padding the context with marginally relevant docs increases cost and can dilute the model's focus. Less, but more relevant, context outperforms large, unfocused context.

**2. Recency bias — put the most important things last**
LLMs have better recall of content near the end of the context (and the very beginning). For long contexts, place your critical instructions, constraints, and the user's actual question at the end — not buried in the middle.

**3. Structure for scannability**
Models, like humans, parse structured content better. Use headers, bullet points, and labeled sections. An unstructured wall of text degrades performance even with the same information.

**4. Context compression (summarization)**
When conversations grow long, compress older turns into a concise summary before they consume too much of the window. Keep recent turns verbatim; summarize older ones. This is what `/compact` does in Claude Code.

**5. Selective retrieval (RAG)**
Instead of embedding entire knowledge bases in context, use retrieval to fetch only the top-K most relevant chunks at query time. This keeps context tight and targeted — the foundation of every production RAG system.

**6. Hierarchical context loading**
Load context progressively:
- Always-present: system prompt, core rules
- Session-level: user profile, conversation history summary
- Task-level: relevant code files, retrieved docs
- Query-level: the specific question or instruction

**7. Explicit > implicit**
Don't rely on the model inferring things it could be told directly. Every assumption you leave unspoken is a potential failure point. State constraints, output format, and edge case handling explicitly.

#### Context Layers in Practice

```
┌─────────────────────────────────────────────────┐
│  System Prompt         (always present)         │
│  Role, rules, output format, hard constraints   │
├─────────────────────────────────────────────────┤
│  Memory / Long-term Facts  (session-level)      │
│  User preferences, past decisions, project info │
├─────────────────────────────────────────────────┤
│  Retrieved Knowledge   (query-time RAG)         │
│  Top-K relevant docs, code snippets, records    │
├─────────────────────────────────────────────────┤
│  Conversation History  (recent turns verbatim)  │
│  Older turns → compressed summary               │
├─────────────────────────────────────────────────┤
│  Current Task / User Message   (bottom = best   │
│  recall position)                               │
└─────────────────────────────────────────────────┘
```

#### Token Budget Management

| Component | Typical Allocation |
|-----------|-------------------|
| System prompt | 1–5% |
| Memory/facts | 5–10% |
| Retrieved context (RAG) | 20–40% |
| Conversation history | 20–30% |
| Current input | 5–10% |
| Reserved for output | 10–25% |

Going over budget forces either truncation (losing information) or spilling into a slower, more expensive model tier.

#### Anti-Patterns to Avoid

- **Dumping entire files** when only a few functions are relevant — use targeted retrieval or file sections
- **Repeating the system prompt** in every user message — the model already has it
- **Ignoring conversation compression** — letting history grow unbounded until the window fills up
- **Burying the actual question** at the top under pages of context — put it at the end
- **Using stale context** — outdated code snippets or docs in context produce confidently wrong answers
- **No explicit output format** — the model will invent its own, leading to inconsistent parsing

#### Context Engineering vs Prompt Engineering vs RAG

| Concept | Focus |
|---------|-------|
| **Prompt Engineering** | How instructions are phrased and structured |
| **Context Engineering** | What information enters the window, how much, in what order |
| **RAG** | Mechanism for dynamically fetching relevant context at query time |
| **Fine-tuning** | Baking knowledge into model weights instead of passing it at runtime |

Context engineering is the layer that orchestrates all of these together in production systems.

---

## ✨ Generative AI (Gen AI) Concepts

- **Generative AI:** A category of artificial intelligence that can create completely new content, such as text, images, code, or audio, based on patterns it learned during its training phase.
- **Tokens:** The basic unit of data processed by an LLM. A token can be a full word, a syllable, or just a single character. Pricing and memory limits (context windows) in AI models are often based on tokens.
- **Hallucination:** A phenomenon where an AI model confidently generates incorrect, fabricated, or entirely nonsensical information as if it were factual.
- **RAG (Retrieval-Augmented Generation):** A framework that makes AI responses significantly more accurate by first searching for factual information in an external database and feeding that context to the AI model before it answers.
- **AI Agents:** Advanced AI implementations that can autonomously plan sequences of actions, make decisions, and use interconnected tools (like web search, API connections, or executing code) to solve complex problems.

---

## ⚙️ n8n & Workflow Automation

**n8n** is a powerful, node-based workflow automation tool that lets you integrate various services and APIs to build complex automated processes via a visual canvas.

### Core n8n Terminology

- **Workflow:** The complete, visual process built in n8n by connecting different actions. It automates tasks by transferring data between normally disconnected apps, services, and databases.
- **Node:** The fundamental building block of a workflow. Each node performs one specific action—such as processing data, interacting with an external API (like Slack or Gemini), or altering logic.
- **Trigger Node:** A specialized node that starts a workflow automatically. Workflows can be triggered by external events (Webhooks, form submissions, incoming emails) or on a scheduled time (CRON jobs).
- **Execution:** A single run of a workflow. Every time a Trigger initiates a workflow, n8n processes the input data through the nodes and logs the result as an Execution.
- **Connection (or Wire):** The visual line connecting two nodes. It dictates the sequence of operations and passes JSON data from the output of one node to the input of the next.
- **Credentials:** A secure way to store authentication details (like API Keys, OAuth tokens, or passwords) needed for nodes to communicate with third-party software securely without exposing secrets directly in the workflow logic.
- **n8n Data Tables (Local DB):** Built-in storage within an n8n environment, allowing workflows to store, append, filter, and retrieve persistent tabular data easily (useful when building custom memory or RAG apps).
