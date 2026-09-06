# User & Interviewer Guide

This guide explains how someone can understand and use the project without reading the source code first.

## 1. What should I do first?

Open the repository README to understand the project, architecture, technology stack, and setup options.

Then follow this guide if you want to actually run the application.

## 2. Prerequisites

You need:

- Python 3.11 (recommended)
- Git
- An LLM provider API key with available usage/credits
- OpenObserve credentials if you want to view traces

Docker can be used instead of local Python setup.

## 3. Local Setup

### Step 1 — Clone the repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd ai-rag-observability-demo
```

### Step 2 — Create a virtual environment

Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

macOS/Linux:

```bash
python3.11 -m venv .venv
source .venv/bin/activate
```

### Step 3 — Configure secrets

Copy `.env.example` to `.env` and provide your own credentials.

**Never commit `.env` to GitHub.**

### Step 4 — Start the LangChain backend

```bash
cd langchain-backend
pip install -r requirements.txt
python -m uvicorn app:app --host 0.0.0.0 --port 8001
```

Check:

```text
http://localhost:8001/health
```

Expected response:

```json
{"status":"ok"}
```

### Step 5 — Configure the frontend

Copy:

```text
frontend/config.example.json
```

to:

```text
frontend/config.json
```

For LangChain:

```json
{
  "BACKEND_URL": "http://localhost:8001"
}
```

### Step 6 — Start the frontend

Open a second terminal:

```bash
cd frontend
python -m http.server 3000
```

Open:

```text
http://localhost:3000
```

## 4. How an End User Uses It

1. Open the web application.
2. Confirm the selected backend is shown in the interface.
3. Enter a question related to the supplied knowledge base.
4. Click the submit/send action.
5. Read the generated answer.
6. Review the displayed sources.
7. Note the trace ID if you want to investigate the request.
8. Select thumbs-up or thumbs-down to provide feedback.

## 5. What Happens Behind the Scenes?

When the user submits a question:

```text
Question
   ↓
Frontend
   ↓
FastAPI /chat
   ↓
Retriever searches relevant document chunks
   ↓
Retrieved context + question
   ↓
LLM
   ↓
Generated answer
   ↓
Answer + sources + trace ID
   ↓
Frontend
```

At the same time, telemetry is generated and exported through OpenTelemetry to OpenObserve.

## 6. How an Interviewer Can Evaluate the Project

An interviewer does not need to understand every file to evaluate the portfolio project.

Recommended order:

1. Read `README.md` for the project overview.
2. Read `docs/PRODUCT_CASE_STUDY.md` for the product problem, user journey, decisions, limitations, and roadmap.
3. Read `docs/architecture.md` for the technical architecture.
4. Run the application using this guide if hands-on evaluation is required.
5. Review `langchain-backend/app.py` to see the API and RAG workflow.
6. Review `telemetry.py` to see how OpenTelemetry is configured.
7. Review `frontend/app.js` to see the user interaction and API integration.

## 7. Testing the API Directly

### Health check

```http
GET http://localhost:8001/health
```

### Chat request

```http
POST http://localhost:8001/chat
Content-Type: application/json

{
  "question": "What is OpenObserve?"
}
```

The response contains:

- `answer`
- `sources`
- `trace_id`

### Feedback

```http
POST http://localhost:8001/feedback
Content-Type: application/json

{
  "trace_id": "<trace-id>",
  "rating": "up",
  "comment": "Helpful answer"
}
```

## 8. Observability Walkthrough

To inspect observability:

1. Configure a reachable OpenObserve instance.
2. Start the backend.
3. Submit a question from the UI.
4. Copy the returned trace ID.
5. Open the OpenObserve trace/observability interface.
6. Locate the corresponding service and trace.
7. Inspect request and AI-related telemetry.
8. Submit feedback and inspect the related feedback span.

## 9. Docker Option

From the repository root:

```bash
docker compose --profile langchain up --build
```

The frontend is available at:

```text
http://localhost:3000
```

## 10. Common Issues

### `Backend config unavailable`

Do not open `index.html` directly. Start the frontend HTTP server and open `http://localhost:3000`.

### `/chat` returns an LLM error

Check that the LLM API key is configured and that the provider account has available usage/credits.

### Traces are missing

Check the OpenObserve URL, organization, authentication token, and network accessibility.

### Windows PowerShell activation error

If script execution is restricted, use the appropriate PowerShell execution-policy configuration for your environment or run the project from a terminal that permits virtual-environment activation.

## 11. Security Reminder

The repository contains placeholders only. Before running the project, the operator must provide their own credentials.

Never commit:

- API keys
- passwords
- OpenObserve authentication tokens
- private credentials
- production environment files
