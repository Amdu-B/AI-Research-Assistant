# 🔬 AI Research Assistant — LLM + RAG + Agentic Reasoning

> An autonomous research assistant that combines **LLaMA 3** (via Groq), **Retrieval-Augmented Generation**, and a **ReAct AI Agent** — all in a single, well-documented Jupyter Notebook.

---

## 🧠 What This Project Does

Most AI demos pick one paradigm. This project integrates three:

| Paradigm | Role |
|---|---|
| **LLM (LLaMA 3 via Groq)** | Reasoning, summarization, and answer generation |
| **RAG (HuggingFace + FAISS)** | Grounds answers in real documents via semantic search |
| **AI Agent (ReAct)** | Autonomously decides *which tool to use* — no hardcoded logic |

The agent receives a question and independently chooses whether to search its document knowledge base, summarize from general knowledge, or introspect its own resources — then generates a cited, grounded answer.

---

## 🏗️ Architecture

```
User Query
    │
    ▼
┌─────────────────────────────────┐
│        AI Agent (LLaMA 3)       │
│  Reason → Act → Observe (ReAct) │
└────────────┬────────────────────┘
             │
    ┌────────┴──────────────┐
    │                       │
    ▼                       ▼
┌──────────┐         ┌─────────────┐
│ RAG Tool │         │  Summarizer │
│  FAISS   │         │  (LLM only) │
│ + HF Emb │         └─────────────┘
└────┬─────┘
     │
Retrieved Chunks → LLM → Final Answer (with source citation)
```

---

## ⚙️ Tech Stack

| Component | Technology |
|---|---|
| LLM | `LLaMA 3 70B` via [Groq](https://console.groq.com) |
| Embeddings | `sentence-transformers/all-MiniLM-L6-v2` (HuggingFace) |
| Vector Store | `FAISS` (Facebook AI Similarity Search) |
| Agent Framework | `LangChain` — ReAct agent |
| Notebook | `Jupyter` `.ipynb` |
| Language | `Python 3.10+` |

---

## 📁 Repository Structure

```
📦 ai-research-assistant
 ┣ 📓 research_assistant_agent.ipynb   ← Main notebook (all code here)
 ┗ 📄 README.md
```

---

## 🚀 Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/YOUR_USERNAME/ai-research-assistant.git
cd ai-research-assistant
```

### 2. Install dependencies

```bash
pip install langchain langchain-community langchain-groq langchain-huggingface \
            faiss-cpu sentence-transformers groq python-dotenv tiktoken colorama
```

Or run **Section 0** of the notebook — it installs everything automatically.

### 3. Get a free Groq API key

Sign up at [console.groq.com](https://console.groq.com) → Create API Key (free tier available).

### 4. Set your API key

Either create a `.env` file:

```
GROQ_API_KEY=gsk_your_key_here
```

Or paste it directly into the notebook config cell:

```python
GROQ_API_KEY = "gsk_your_key_here"
```

### 5. Run the notebook

```bash
jupyter notebook research_assistant_agent.ipynb
```

Run all cells top to bottom. The first run downloads the HuggingFace embedding model (~90 MB).

---

## 📓 Notebook Walkthrough

| Section | Description |
|---|---|
| **0 — Installation** | One-cell pip install for all dependencies |
| **1 — Configuration** | API keys, model names, hyperparameters |
| **2 — Document Ingestion & RAG** | Chunking, HuggingFace embeddings, FAISS index |
| **3 — Groq LLM Setup** | LLaMA 3 initialisation + sanity check |
| **4 — Agent Tools** | `rag_search`, `summarize_topic`, `inspect_knowledge_base` |
| **5 — Agent Construction** | ReAct agent via LangChain Hub prompt |
| **6 — Interactive Demo** | 5 queries showcasing RAG, general LLM, and meta tool use |
| **7 — Evaluation** | Keyword-precision scoring + retrieval inspection |

---

## 💬 Example Agent Interactions

```
❓ What are the key components of the Transformer architecture?
   → Agent uses: rag_search
   → Retrieves from: transformers_overview.txt
   → Answer cites source ✅

❓ What topics do you have in your knowledge base?
   → Agent uses: inspect_knowledge_base
   → Returns: chunk count, topics, source files ✅

❓ Give me an overview of RLHF.
   → Agent uses: summarize_topic (not in documents)
   → LLM answers from general knowledge ✅
```

---

## 📊 Evaluation

The notebook includes a lightweight **keyword-precision evaluator** that checks whether expected terms appear in RAG answers — a quick proxy for factual accuracy without needing a labelled dataset.

For production evaluation, integrate [RAGAS](https://github.com/explodinggradients/ragas) which measures:
- **Faithfulness** — is the answer supported by retrieved docs?
- **Answer Relevance** — does the answer address the question?
- **Context Precision / Recall** — is retrieval accurate and complete?

---

## 🚀 Production Upgrade Path

- **Better retrieval** → HyDE, query rewriting, cross-encoder re-ranking
- **More documents** → `PyPDFLoader`, `WebBaseLoader`, `GitbookLoader`
- **Persistent index** → Pinecone / Weaviate / Qdrant instead of in-memory FAISS
- **Conversation memory** → `ConversationBufferMemory` for multi-turn chat
- **Multi-agent** → Add a Critic agent to validate answers before returning
- **Streaming** → `StreamingStdOutCallbackHandler` for real-time token output
- **Web UI** → Wrap with Gradio or Streamlit

---

## 📚 References

- Vaswani et al. (2017) — [Attention Is All You Need](https://arxiv.org/abs/1706.03762)
- Lewis et al. (2020) — [Retrieval-Augmented Generation](https://arxiv.org/abs/2005.11401)
- Yao et al. (2022) — [ReAct: Synergizing Reasoning and Acting](https://arxiv.org/abs/2210.03629)
- [LangChain Documentation](https://python.langchain.com)
- [Groq Console](https://console.groq.com)
- [RAGAS Evaluation Framework](https://github.com/explodinggradients/ragas)

---

## 📝 License

MIT License — free to use, modify, and distribute.

---

<p align="center">Built with 🦙 LLaMA 3 · ⚡ Groq · 🤗 HuggingFace · 🦜 LangChain</p>
