# AI RAG Observability Demo

> An end-to-end AI portfolio project demonstrating **RAG + LLM integration + user feedback + LLM observability** with LangChain, LlamaIndex, OpenTelemetry, and OpenObserve.

**Portfolio focus:** AI Product Management / AI application design / AI observability

> **Attribution:** This project is adapted from the OpenObserve LangChain/LlamaIndex tracing demo. The original Apache 2.0 license and attribution are retained. Portfolio-specific work includes configuration handling, documentation, frontend/backend setup guidance, and developer-experience improvements.

## 1. What is this project?

This project demonstrates how an AI knowledge assistant can be built and observed end-to-end.

A user asks a question through a simple web interface. The backend retrieves relevant information from a controlled knowledge base, sends the retrieved context and question to an LLM, and returns an answer with source information and a trace ID.

In parallel, OpenTelemetry/OpenLLMetry captures telemetry that can be inspected in OpenObserve. User feedback is also recorded and linked to the interaction.

### The core workflow

```text
User Question
     ↓
Web Frontend
     ↓
FastAPI Backend
     ↓
RAG Retrieval
     ↓
Relevant Context + Question
     ↓
LLM
     ↓
Answer + Sources + Trace ID
     ↓
User Feedback
     ↓
OpenTelemetry → OpenObserve
```

## 2. Why this is an AI Product Portfolio Project

The project connects the **user experience** with the **AI system behind it**.

It provides a practical basis for discussing:

- AI/RAG user journeys
- Retrieval and grounding
- LLM integration
- Feedback loops
- AI observability
- Quality and reliability signals
- Product trade-offs
- Future AI evaluation and experimentation

For the product-oriented explanation, see [`docs/PRODUCT_CASE_STUDY.md`](docs/PRODUCT_CASE_STUDY.md).

## 3. Key Features

- RAG-based question answering
- LangChain backend implementation
- LlamaIndex backend implementation
- Local embeddings using FastEmbed
- FAISS vector search for LangChain
- LlamaIndex local storage/indexing
- LLM integration with Anthropic Claude
- OpenTelemetry/OpenLLMetry tracing
- OpenObserve telemetry export
- Trace ID returned to the UI
- Thumbs-up/down feedback
- Docker Compose support
- Clear local setup and interviewer guide

## 4. Architecture

```text
                         ┌──────────────────────┐
                         │      Web Frontend     │
                         │   HTML / CSS / JS     │
                         └──────────┬───────────┘
                                    │ HTTP
                                    ▼
                    ┌──────────────────────────────┐
                    │        FastAPI Backend       │
                    │  LangChain OR LlamaIndex     │
                    └──────────────┬───────────────┘
                                   │
                     ┌─────────────┴─────────────┐
                     │                           │
                     ▼                           ▼
             ┌──────────────┐            ┌──────────────┐
             │ RAG Retrieval│            │     LLM      │
             │ Embeddings + │            │    Claude    │
             │ Vector Index  │            └──────────────┘
             └──────────────┘                    │
                     │                           │
                     └─────────────┬─────────────┘
                                   ▼
                         ┌─────────────────────┐
                         │   OpenTelemetry     │
                         │  + OpenLLMetry      │
                         └──────────┬──────────┘
                                    │ OTLP/HTTP
                                    ▼
                         ┌─────────────────────┐
                         │    OpenObserve      │
                         │  Trace Exploration  │
                         └─────────────────────┘
```

## 5. Technology Stack

| Layer | Technology |
|---|---|
| Frontend | HTML, CSS, JavaScript |
| API | FastAPI, Uvicorn |
| RAG | LangChain / LlamaIndex |
| LLM | Anthropic Claude |
| Embeddings | FastEmbed / BAAI BGE-small-en-v1.5 |
| Vector store | FAISS for LangChain |
| Observability | OpenTelemetry + OpenLLMetry |
| Telemetry backend | OpenObserve |
| Containerization | Docker / Docker Compose |

## 6. Repository Structure

```text
.
├── frontend/                    # Browser chat application
├── langchain-backend/           # LangChain RAG API
├── llamaindex-backend/          # LlamaIndex RAG API
├── docs/
│   ├── PRODUCT_CASE_STUDY.md    # Product/PM-oriented documentation
│   ├── USER_GUIDE.md            # Step-by-step user/interviewer guide
│   ├── architecture.md          # Technical architecture
│   └── trace-examples.md        # Trace examples
├── docker-compose.yml
├── run.sh
├── .env.example
├── .python-version
└── LICENSE
```

## 7. Documentation Map

| Document | Purpose |
|---|---|
| `README.md` | First entry point: project, architecture, setup, and portfolio context |
| `docs/PRODUCT_CASE_STUDY.md` | AI PM-style problem, user journey, decisions, limitations, metrics, and roadmap |
| `docs/USER_GUIDE.md` | Step-by-step instructions for running and evaluating the application |
| `docs/architecture.md` | Technical architecture and telemetry flow |
| `docs/trace-examples.md` | Examples of expected trace behavior |

## 8. Quick Start

### Prerequisites

Recommended:

- Python 3.11
- Git
- An LLM API key with available usage/credits
- OpenObserve cloud or self-hosted instance for trace inspection

Docker Desktop can be used instead of local Python execution.

### Step 1 — Clone

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd ai-rag-observability-demo
```

### Step 2 — Configure environment

Copy `.env.example` to `.env`.

Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

macOS/Linux:

```bash
cp .env.example .env
```
Generate your OPENOBSERVE_AUTH_TOKEN:

echo -n "you@example.com:yourpassword" | base64
Then set it in .env as:

OPENOBSERVE_AUTH_TOKEN=Basic <base64-value>
Add your own credentials to `.env`.

**Never commit `.env`.**

### Step 3 — Run LangChain backend

```bash
cd langchain-backend
pip install -r requirements.txt
python -m uvicorn app:app --host 0.0.0.0 --port 8001
```

### Step 4 — Configure frontend

Copy `frontend/config.example.json` to `frontend/config.json` and point it to:

```json
{
  "BACKEND_URL": "http://localhost:8001"
}
```

### Step 5 — Run frontend

In a second terminal:

```bash
cd frontend
python -m http.server 3000
```

Open:

```text
http://localhost:3000
```

**Do not open `index.html` directly with `file://`.**

For the complete guide, see [`docs/USER_GUIDE.md`](docs/USER_GUIDE.md).

## 9. Docker Quick Start

From the repository root, after creating `.env`:

```bash
docker compose --profile langchain up --build
```

For LlamaIndex:

```bash
docker compose --profile llamaindex up --build
```

Frontend:

```text
http://localhost:3000
```

LangChain API:

```text
http://localhost:8001
```

LlamaIndex API:

```text
http://localhost:8002
```

## 10. How an End User Uses the Product

1. Open the web application.
2. Enter a question about the supplied knowledge base.
3. Submit the question.
4. The application retrieves relevant information.
5. The LLM generates a grounded answer.
6. The UI displays the answer and sources.
7. A trace ID is available for investigation.
8. The user can provide thumbs-up/down feedback.

## 11. How an Interviewer Can Evaluate It

Recommended reading order:

1. Read this README for the product and architecture overview.
2. Read [`docs/PRODUCT_CASE_STUDY.md`](docs/PRODUCT_CASE_STUDY.md) for product thinking, decisions, limitations, and roadmap.
3. Read [`docs/USER_GUIDE.md`](docs/USER_GUIDE.md) to run the application.
4. Review `langchain-backend/app.py` for the API/RAG workflow.
5. Review `langchain-backend/telemetry.py` for observability configuration.
6. Review `frontend/app.js` for frontend/backend interaction.

## 12. API

### Health

```http
GET /health
```

Returns:

```json
{"status":"ok"}
```

### Chat

```http
POST /chat
Content-Type: application/json

{"question":"What is OpenObserve?"}
```

Response includes:

- `answer`
- `sources`
- `trace_id`

### Feedback

```http
POST /feedback
Content-Type: application/json

{
  "trace_id":"<trace-id>",
  "rating":"up",
  "comment":"Helpful answer"
}
```

## 13. Observability Flow

For each chat request:

1. Frontend sends the question to FastAPI.
2. RAG retrieves relevant document chunks.
3. Retrieved context is combined with the question.
4. LLM generates the answer.
5. OpenTelemetry/OpenLLMetry captures telemetry.
6. OTLP/HTTP exports traces to OpenObserve.
7. API returns answer, sources, and trace ID.
8. Feedback is recorded as a linked telemetry event.

## 14. Security

The repository contains **placeholders only**. No real API keys, passwords, OpenObserve credentials, or access tokens should be committed.

Secrets are supplied through environment variables such as:

- `ANTHROPIC_API_KEY`
- `OPENOBSERVE_AUTH_TOKEN`

If a credential is ever accidentally committed, revoke/rotate it immediately and remove it from Git history before making the repository public.

## 15. Limitations & Future Roadmap

This is a portfolio/reference implementation, not a production SaaS product.

Potential next steps:

- Persistent vector database
- Authentication and multi-tenancy
- Persistent feedback analytics
- Retrieval/answer quality evaluation
- Prompt and model versionings
- Cost and latency monitoring
- AI quality dashboards
- Human review workflows
- Automated evaluation and regression testing

See [`docs/PRODUCT_CASE_STUDY.md`](docs/PRODUCT_CASE_STUDY.md) for the full roadmap.

## 16.License
License: Apache License 2.0. See [`LICENSE`](LICENSE).
