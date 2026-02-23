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
