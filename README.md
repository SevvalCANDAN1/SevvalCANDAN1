<div align="center">

# Şevval CANDAN

**AI & Data Engineering · FastAPI RAG · LLM backends**

![Typing SVG](https://readme-typing-svg.herokuapp.com?font=Fira+Code&size=20&duration=2800&pause=900&color=818CF8&center=true&vCenter=true&width=720&lines=FastAPI+%2B+Elasticsearch+RAG;BYOK+keys+never+hit+the+database;Retrieve+k%3D3%2C+then+cite+the+page)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/sevval-candan1/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/SevvalCANDAN1)
[![LeetCode](https://img.shields.io/badge/LeetCode-FFA116?style=for-the-badge&logo=leetcode&logoColor=black)](https://leetcode.com/u/Sevval_CANDAN/)

</div>

> Retrieve first. Cite the page. Do not invent.

Ankara University — Computer Engineering, double major **AI & Data Engineering** (GPA **3.45**). Former Backend & AI research intern at **TRMiX** (through Sep 2026). Main work: **RAG services** — FastAPI at the edge, Elasticsearch for vectors, LangChain for the chain, keys that never land in the DB.

```python
class Focus:
    intern = "TRMiX · Backend & AI research (ended Sep 2026)"
    now = "fastapi-rag-assistant  # public, still growing"
    loop = ["PDF ingest", "embed", "retrieve k=3", "generate + sources"]
    serve = "FastAPI /rag/v1 + Vite/React (Folio)"
    store = "Elasticsearch  (workspace-isolated indices)"
```

---

## 1. FastAPI RAG Assistant — the current system

[github.com/SevvalCANDAN1/fastapi-rag-assistant](https://github.com/SevvalCANDAN1/fastapi-rag-assistant) · MIT · **WIP** (catalog is ahead of the query chain)

Public FastAPI RAG: upload a PDF, answer only from retrieved chunks, show **filename + page**. React UI (`frontend/`, Folio). Elasticsearch is both vector store and workspace settings. API keys travel as **BYOK headers** — not stored.

```text
Browser (Folio)                    FastAPI  /rag/v1
 Setup → workspace + optional key        │
      → chat / docs / instructions       ├─ POST /documents/index
            │                            ├─ POST /query
     demo (no network) or live ──────────┼─ GET/PUT /prompt
                                         ├─ GET  /catalog/*
                                         └─ GET  /health  (ES ping → 503 if down)
                                              │
                                    services/loader.py     PDF → chunks 500/50
                                    services/elastic_rag.py  embed · retrieve · LLM
                                              │
                                    Elasticsearch
                                      rag-{workspace}           vectors
                                      rag-workspace-settings    prompt / model prefs
```

| Piece | What shipped |
|:------|:-------------|
| **HTTP** | Versioned `/rag/v1`. Routers own headers, 10 MB PDF cap, temp file cleanup. `services/` owns RAG. Thin `main.py`. |
| **Index** | `PyPDFLoader` → `RecursiveCharacterTextSplitter` (500 / 50) → Gemini embeddings → `rag-{workspace}`. |
| **Query** | Retriever **k=3**. Context tagged `[filename p.N]`. LCEL: system prompt + context + question → `gemini-2.5-flash`. `source_documents` in the JSON so the UI can cite. |
| **BYOK** | `X-Workspace-Id`, `X-Llm-Api-Key` / `X-Embedding-Api-Key` (Gemini header still accepted). Key stays in `sessionStorage` on the client. CORS allowlists those headers. |
| **Workspaces** | One ES index per id (`rag-{sanitized}`). Users on the same cluster do not share documents. Prompt lives in a separate settings index. |
| **Health** | `/` = process up. `/rag/v1/health` = Elasticsearch ping. Degraded store is **503**, not a fake 200. |
| **UI** | Vite + React on :3000. Same `ApiClient` for demo mode (no API) and live. Sources in a side rail, not buried in the chat bubble. |
| **Run** | `uvicorn main:app`; `docker compose` for local ES 9; Procfile for Render. |

Honest gaps (written in the repo’s `ARCHITECTURE.md`, not hidden): query path is still Gemini-fixed even though the **model catalog** (OpenAI / Anthropic / Groq / Voyage) exists; no streaming; PDF only; identity is the workspace header, not JWT yet.

```http
POST /rag/v1/query
X-Workspace-Id: demo
X-Llm-Api-Key:  (yours, not mine)

{ "question": "Bu belgede SLA kaç gün?" }
```

---

## How the rest of the AI work fits

Same rule — **context before generation** — in different shapes:

### Eco-Smart Inference Layer
Turkcell Yarının Teknoloji Liderleri · semifinal · with Aslınur Demir & Zehra Vural  

Semantic **cache** in front of the LLM: embed the query, hybrid search (FAISS / Chroma / Elasticsearch), return a hit (~16×) or call the model. FastAPI + LangChain. Complementary to Folio: Folio answers from *documents*; Eco-Smart skips the model when the *question* already happened.

### [PDF RAG chatbot](https://github.com/SevvalCANDAN1/RAG_PROJECT)
Earlier document loop: Streamlit, MiniLM, local FAISS, Gemini 2.5 Flash, chunk 5000 / 500. The index without the HTTP product. Folio is that pipeline behind FastAPI + Elasticsearch + citations.

### [Grafana AI Dashboard Orchestrator](https://github.com/SevvalCANDAN1/grafana-llm-builder)
Schema-as-context: discover Grafana datasources and PostgreSQL columns, prompt an Ollama-compatible LLM, push valid dashboard JSON. No invented fields.

### TRMiX — Backend & AI research intern (completed Sep 2026)
Async FastAPI, prompt work, Chroma / Elasticsearch next to production traffic.

### Also AI
TEKNOFEST 2026 Health (variant classification) · [freelancer model](https://github.com/SevvalCANDAN1/Data-Science-P) (XGBoost / SMOTE) · [YDS NLP API](https://github.com/SevvalCANDAN1/homework) (FastAPI + spaCy — not vector RAG).

---

## Stack I reach for first

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=for-the-badge&logo=elasticsearch&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini-8E75B2?style=for-the-badge&logo=googlegemini&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![FAISS](https://img.shields.io/badge/FAISS-FFE800?style=for-the-badge)
![Chroma](https://img.shields.io/badge/ChromaDB-FF6F57?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

</div>

HuggingFace MiniLM, Streamlit, spaCy, scikit-learn, XGBoost, PostgreSQL, AWS, Django, Node, Kotlin when the problem needs them.

---

<details>
<summary><strong>Course / product work (not the main story)</strong></summary>

<br/>

AWS IoT city sensors ([homework-2](https://github.com/SevvalCANDAN1/homework-2)) · e-commerce API on ALB/ASG/RDS ([homework-1](https://github.com/SevvalCANDAN1/homework-1)) · Django quality monitor ([Turkcell](https://github.com/SevvalCANDAN1/Turkcell)) · expense tracker Compose ([cloud](https://github.com/SevvalCANDAN1/cloud)) · SayfaSayfa store ([sayfasayfa](https://github.com/SevvalCANDAN1/sayfasayfa)) · Compose Android labs · SQL / pandas / C structures.

</details>
