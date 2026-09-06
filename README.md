# AI RAG Observability Demo

> An end-to-end AI portfolio project demonstrating RAG, LLM integration, user feedback, and LLM observability using LangChain, LlamaIndex, OpenTelemetry, OpenLLMetry, and OpenObserve.

**Portfolio focus:** AI Product Management · AI Application Design · RAG · AI Observability

> **Attribution:** This project is adapted from the [OpenObserve LangChain/LlamaIndex Tracing Demo](https://github.com/openobserve/langchain-llamaindex-tracing-demo). The original Apache 2.0 license and attribution are retained. Portfolio-specific work includes project documentation, product-oriented documentation, configuration guidance, setup improvements, and developer/interviewer experience improvements.

---

## 1. Project Overview

This project demonstrates how an AI-powered knowledge assistant can be built, deployed locally, and observed end-to-end.

A user submits a question through a web interface. The backend retrieves relevant information from a knowledge base, combines the retrieved context with the user's question, sends it to an LLM, and returns a grounded response with source information and a trace ID.

At the same time, OpenLLMetry and OpenTelemetry capture telemetry from the AI workflow and export traces to OpenObserve for investigation and monitoring.

Users can also provide thumbs-up or thumbs-down feedback, allowing feedback to be associated with the corresponding AI interaction.

### Core Workflow

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
LLM (Claude)
      ↓
Answer + Sources + Trace ID
      ↓
User Feedback
      ↓
OpenLLMetry
      ↓
OpenTelemetry
      ↓
OpenObserve
```

---

## 2. Why This Project?

This project is designed as an AI Product Management and AI application portfolio project.

It demonstrates how the **user experience**, **AI system**, and **observability layer** work together.

It provides a practical foundation for discussing:

- AI/RAG user journeys
- Retrieval and grounding
- LLM integration
- AI application architecture
- User feedback loops
- AI observability
- Trace-based debugging
- Quality and reliability signals
- Product trade-offs
- AI evaluation opportunities
- Future product roadmap

For the product-oriented analysis, see:

`docs/PRODUCT_CASE_STUDY.md`

---

## 3. Key Features

- RAG-based question answering
- LangChain backend
- LlamaIndex backend
- Anthropic Claude integration
- Local embeddings using FastEmbed
- `BAAI/bge-small-en-v1.5` embeddings
- FAISS vector search for LangChain
- LlamaIndex local indexing/storage
- OpenLLMetry instrumentation
- OpenTelemetry tracing
- OTLP/HTTP export
- OpenObserve trace visualization
- Trace ID returned to the frontend
- Thumbs-up/down user feedback
- Docker Compose support
- Separate LangChain and LlamaIndex backend options
- Product case study and architecture documentation
- Interviewer/user setup guide

---

# 4. Architecture

```text
                         ┌──────────────────────┐
                         │      Web Frontend     │
                         │   HTML / CSS / JS     │
                         └──────────┬───────────┘
                                    │ HTTP
                                    ▼
                         ┌──────────────────────┐
                         │    FastAPI Backend   │
                         │ LangChain / LlamaIndex│
                         └──────────┬───────────┘
                                    │
                       ┌────────────┴────────────┐
                       │                         │
                       ▼                         ▼
               ┌──────────────┐          ┌──────────────┐
               │ RAG Retrieval│          │     LLM      │
               │ Embeddings + │          │   Claude     │
               │ Vector Index │          └──────┬───────┘
               └──────────────┘                 │
                       │                        │
                       └────────────┬───────────┘
                                    ▼
                         ┌──────────────────────┐
                         │     OpenLLMetry      │
                         │  Instrumentation     │
                         └──────────┬───────────┘
                                    ▼
                         ┌──────────────────────┐
                         │   OpenTelemetry SDK  │
                         │   OTLP HTTP Export   │
                         └──────────┬───────────┘
                                    ▼
                         ┌──────────────────────┐
                         │     OpenObserve      │
                         │  Trace Exploration   │
                         └──────────────────────┘
```

### Observability Pipeline

The observability layer consists of four main stages:

1. **Application layer**
   LangChain or LlamaIndex executes the RAG workflow.

2. **OpenLLMetry instrumentation**
   AI framework and LLM operations are automatically instrumented.

3. **OpenTelemetry SDK**
   Telemetry is processed and exported through OTLP.

4. **OpenObserve**
   Traces and spans are stored and explored for debugging and monitoring.

---

# 5. Technology Stack

| Layer            | Technology                         |
| ---------------- | ---------------------------------- |
| Frontend         | HTML, CSS, JavaScript              |
| API              | FastAPI, Uvicorn                   |
| RAG              | LangChain / LlamaIndex             |
| LLM              | Anthropic Claude                   |
| Embeddings       | FastEmbed / BAAI BGE-small-en-v1.5 |
| Vector Search    | FAISS                              |
| Instrumentation  | OpenLLMetry                        |
| Telemetry        | OpenTelemetry                      |
| Observability    | OpenObserve                        |
| Containerization | Docker / Docker Compose            |
| Language         | Python                             |

---

# 6. Repository Structure

```text
.
├── frontend/
│   ├── index.html
│   ├── app.js
│   └── ...
│
├── langchain-backend/
│   ├── app.py
│   ├── telemetry.py
│   ├── requirements.txt
│   └── ...
│
├── llamaindex-backend/
│   ├── app.py
│   ├── telemetry.py
│   ├── requirements.txt
│   └── ...
│
├── docs/
│   ├── PRODUCT_CASE_STUDY.md
│   ├── USER_GUIDE.md
│   ├── architecture.md
│   └── trace-examples.md
│
├── docker-compose.yml
├── run.sh
├── .env.example
├── .gitignore
├── LICENSE
└── README.md
```

---

# 7. Documentation

| Document                     | Purpose                                              |
| ---------------------------- | ---------------------------------------------------- |
| `README.md`                  | Project overview, architecture and setup             |
| `docs/PRODUCT_CASE_STUDY.md` | Product thinking, decisions, limitations and roadmap |
| `docs/USER_GUIDE.md`         | Step-by-step application setup and evaluation        |
| `docs/architecture.md`       | Technical architecture and telemetry flow            |
| `docs/trace-examples.md`     | Expected OpenObserve trace examples                  |

---

# 8. Prerequisites

Before running the project, install:

- Python 3.11
- Git
- Docker Desktop (recommended)
- An Anthropic API key
- An OpenObserve account (cloud or self-hosted)

You can run the application either through Docker Compose or directly using Python.

---

# 9. Local Setup

## Step 1 — Clone the repository

```bash
git clone https://github.com/Im-Nitish-kumar/ai-rag-observability-demo.git
cd ai-rag-observability-demo
```

---

## Step 2 — Configure environment variables

Create `.env` from `.env.example`.

### Windows PowerShell

```powershell
Copy-Item .env.example .env
```

### macOS/Linux

```bash
cp .env.example .env
```

Generate your OPENOBSERVE_AUTH_TOKEN:

echo -n "you@example.com:yourpassword" | base64
Then set it in .env as:

Add your own credentials to `.env`.

Typical configuration:

```env
ANTHROPIC_API_KEY=your_anthropic_api_key

OPENOBSERVE_URL=https://api.openobserve.ai
OPENOBSERVE_ORG=your_org_slug
OPENOBSERVE_AUTH_TOKEN=Basic <base64-value>
```

### Generate the OpenObserve authentication token

The token is generated by encoding:

```text
email:password
```

For example:

```bash
echo -n "you@example.com:yourpassword" | base64
```

Then use:

```env
OPENOBSERVE_AUTH_TOKEN=Basic <base64-value>
```

**Do not commit `.env` to GitHub.**

---

# 10. Run Using Docker

Docker is the recommended way to run the complete application.

From the repository root:

### LangChain

```bash
docker compose --profile langchain up --build
```

### LlamaIndex

```bash
docker compose --profile llamaindex up --build
```

The frontend will be available at:

```text
http://localhost:3000
```

LangChain backend:

```text
http://localhost:8001
```

LlamaIndex backend:

```text
http://localhost:8002
```

---

# 11. Run Without Docker

## LangChain Backend

Open a terminal:

```bash
cd langchain-backend
pip install -r requirements.txt
python -m uvicorn app:app --host 0.0.0.0 --port 8001
```

The LangChain API will run on:

```text
http://localhost:8001
```

---

## LlamaIndex Backend

Open another terminal:

```bash
cd llamaindex-backend
pip install -r requirements.txt
python -m uvicorn app:app --host 0.0.0.0 --port 8002
```

The LlamaIndex API will run on:

```text
http://localhost:8002
```

---

# 12. Run the Frontend

Open another terminal:

```bash
cd frontend
python -m http.server 3000
```

Open:

```text
http://localhost:3000
```

Do not open `index.html` directly using:

```text
file://
```

The frontend should be served through a local HTTP server.

---

# 13. Switching Between LangChain and LlamaIndex

The project supports two backend implementations.

### LangChain

```text
Frontend
   ↓
FastAPI
   ↓
LangChain
   ↓
RAG
   ↓
Claude
```

### LlamaIndex

```text
Frontend
   ↓
FastAPI
   ↓
LlamaIndex
   ↓
RAG
   ↓
Claude
```

When using the Docker workflow, select the desired backend profile.

LangChain:

```bash
docker compose --profile langchain up --build
```

LlamaIndex:

```bash
docker compose --profile llamaindex up --build
```

---

# 14. Using the Application

Once the application is running:

1. Open the web application.
2. Enter a question related to the supplied knowledge base.
3. Submit the question.
4. The RAG pipeline retrieves relevant information.
5. The retrieved context is provided to the LLM.
6. The LLM generates a grounded response.
7. The UI displays the answer and sources.
8. A trace ID is returned.
9. The user can provide thumbs-up or thumbs-down feedback.
10. The interaction can then be investigated in OpenObserve.

---

# 15. OpenObserve: What You Can See

For each `/chat` request, OpenObserve can show:

- A trace representing the request
- Retrieval-related spans
- Prompt generation
- LLM/Anthropic API activity
- LLM telemetry attributes
- Token-related information where available
- Trace IDs
- Feedback telemetry
- The relationship between the original interaction and feedback

This makes it possible to investigate questions such as:

- Why did an AI response take longer?
- Was the retrieval step successful?
- Which part of the workflow generated latency?
- How many tokens were used?
- Did the LLM call succeed?
- What happened when a user gave negative feedback?

---

# 16. API Endpoints

## Health Check

```http
GET /health
```

Response:

```json
{
  "status": "ok"
}
```

---

## Chat

```http
POST /chat
Content-Type: application/json
```

Request:

```json
{
  "question": "What is OpenObserve?"
}
```

Response includes:

```text
answer
sources
trace_id
```

---

## Feedback

```http
POST /feedback
Content-Type: application/json
```

Example:

```json
{
  "trace_id": "<trace-id>",
  "rating": "up",
  "comment": "Helpful answer"
}
```

---

# 17. Observability Flow

For every chat request:

```text
1. User submits question
        ↓
2. Frontend sends request to FastAPI
        ↓
3. RAG retrieves relevant document chunks
        ↓
4. Context + question are sent to the LLM
        ↓
5. LLM generates the response
        ↓
6. OpenLLMetry captures AI telemetry
        ↓
7. OpenTelemetry exports traces using OTLP
        ↓
8. OpenObserve receives and stores the traces
        ↓
9. API returns answer + sources + trace ID
        ↓
10. User feedback is recorded and linked to the interaction
```

---

# 18. Troubleshooting

### Traces are not appearing in OpenObserve

Check:

```env
OPENOBSERVE_URL
OPENOBSERVE_ORG
OPENOBSERVE_AUTH_TOKEN
```

Make sure the authentication token uses:

```text
Basic <base64-value>
```

and does not contain unnecessary quotation marks.

---

### 401 Unauthorized

Regenerate the authentication token:

```bash
echo -n "email:password" | base64
```

Then update:

```env
OPENOBSERVE_AUTH_TOKEN=Basic <base64-value>
```

---

### Frontend cannot connect to backend

Verify that the correct backend is running.

LangChain:

```text
http://localhost:8001
```

LlamaIndex:

```text
http://localhost:8002
```

Also verify the frontend's backend URL configuration.

---

### Application starts but AI requests fail

Check that:

```env
ANTHROPIC_API_KEY
```

is correctly configured and that the API key has available usage/credits.

---

### OTLP traces are not exported

Verify that the OpenObserve OTLP endpoint and organization configuration are correct.

The OTLP endpoint should follow the expected OpenObserve format:

```text
/api/<organization>/v1/traces
```

---

# 19. Security

Never commit credentials or secrets to GitHub.

The following should remain local:

```text
.env
```

The repository should only contain:

```text
.env.example
```

Potential secrets include:

```text
ANTHROPIC_API_KEY
OPENOBSERVE_AUTH_TOKEN
```

If a real credential is accidentally committed:

1. Revoke/rotate the credential immediately.
2. Remove the secret from Git history.
3. Generate a new credential.
4. Update the local `.env`.

---

# 20. Product Perspective

From a product perspective, this project demonstrates an AI assistant with an observability feedback loop.

### User Layer

Users ask questions and receive grounded answers.

### AI Layer

The system performs:

- Retrieval
- Context construction
- LLM generation
- Source attribution

### Observability Layer

The system captures:

- Request traces
- AI/LLM operations
- Retrieval activity
- Latency signals
- Token information
- Feedback relationships

### Product Opportunity

The observability data can eventually support:

- AI quality monitoring
- Retrieval evaluation
- Prompt experimentation
- Model comparison
- Cost monitoring
- Latency optimization
- User feedback analytics
- Human review workflows
- Regression testing

---

# 21. Limitations

This is a portfolio/reference implementation and is **not intended to represent a production-ready SaaS platform**.

Current limitations include:

- Local vector storage
- Limited authentication
- No production-grade multi-tenancy
- Limited persistent feedback analytics
- No comprehensive automated evaluation framework
- No production-scale data ingestion pipeline

---

# 22. Future Roadmap

Potential improvements include:

- Persistent vector database
- Authentication and authorization
- Multi-tenancy
- Persistent feedback analytics
- Retrieval/answer quality evaluation
- Prompt and model versioning
- Cost and latency monitoring
- AI quality dashboards
- Human review workflows
- Automated evaluation
- Regression testing
- Production deployment

See [`docs/PRODUCT_CASE_STUDY.md`](docs/PRODUCT_CASE_STUDY.md) for the full roadmap.

## 16. Attribution & License

Original demo concept/source: **OpenObserve LangChain/LlamaIndex tracing demo**.

This repository retains the original Apache 2.0 license and attribution. Portfolio-specific documentation and developer-experience improvements have been added around the reference implementation.

License: Apache License 2.0. See [`LICENSE`](LICENSE).
