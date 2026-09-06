# AI RAG Observability Demo — Product Case Study

## 1. Product Snapshot

**Product:** AI RAG Observability Demo  
**Category:** AI Application / Developer Productivity / LLM Observability  
**Primary user:** Developer, product engineer, AI product manager, or evaluator exploring an AI-powered knowledge assistant.  
**Core problem:** An AI assistant can produce useful answers, but teams also need to understand where answers came from, how AI requests behave, and how users respond to them.

## 2. Problem Statement

Build a small, understandable AI assistant that can answer questions from a controlled knowledge base while making the underlying AI workflow observable.

The product should demonstrate two sides of an AI product:

- **User experience:** Ask a question and receive an answer with source information.
- **Operational experience:** Generate traceable telemetry so a developer/product team can investigate AI interactions and user feedback.

## 3. Product Goal

Create an end-to-end reference implementation that connects an AI chat experience, RAG retrieval, LLM generation, feedback capture, and observability in one workflow.

## 4. User Journey

1. User opens the web application.
2. User enters a question about the knowledge base.
3. Frontend sends the question to the selected FastAPI backend.
4. The RAG layer retrieves the most relevant document chunks.
5. Retrieved context is combined with the user's question.
6. The LLM generates a grounded response.
7. The API returns the answer, source information, and trace ID.
8. User can provide thumbs-up or thumbs-down feedback.
9. Feedback is recorded against the related trace.
10. Developer/product team can inspect telemetry in OpenObserve.

## 5. Why RAG?

A general-purpose LLM may not know the application's private or domain-specific information. RAG provides a simple way to ground an answer in a controlled document set.

In this project:

**Documents → Chunking → Embeddings → Retrieval → Context + Question → LLM → Answer**

The LangChain implementation uses FastEmbed embeddings and FAISS for local similarity search. The LlamaIndex implementation uses its local index/storage approach.

## 6. Product Architecture

```text
User
  |
  v
Web UI (HTML/CSS/JS)
  |
  v
FastAPI /chat
  |
  +----------------------+
  |                      |
  v                      v
RAG Retrieval           LLM
  |                      |
  +----------+-----------+
             |
             v
          Answer
             |
             +--------------------> User
             |
             v
   OpenTelemetry / OpenLLMetry
             |
             v
        OpenObserve
             ^
             |
       User feedback
```

## 7. Key Product Decisions

### Decision 1 — Keep the frontend lightweight

A static frontend makes the demo easy to run, inspect, and deploy without introducing an unnecessary frontend framework.

### Decision 2 — Support two RAG frameworks

The project exposes both LangChain and LlamaIndex implementations. This demonstrates that the observability pattern can sit around different AI application frameworks.

### Decision 3 — Return a trace ID to the user interface

The trace ID creates a bridge between the user-facing request and the operational telemetry. This is useful during debugging and product evaluation.

### Decision 4 — Capture explicit feedback

Thumbs-up/down feedback creates a basic product-quality signal. In a production product, this could be expanded into feedback analytics, evaluation datasets, and model/prompt improvement loops.

## 8. AI/ML Components

- **LLM:** Anthropic Claude in the reference implementation.
- **Embeddings:** `BAAI/bge-small-en-v1.5` through FastEmbed.
- **Retrieval:** FAISS for the LangChain backend; local LlamaIndex storage for the LlamaIndex backend.
- **Frameworks:** LangChain and LlamaIndex.
- **Observability:** OpenTelemetry and OpenLLMetry instrumentation.
- **Telemetry backend:** OpenObserve.

## 9. Success Signals

For a portfolio demonstration, the important success signals are:

- User can successfully submit a question.
- Retrieved context is used to ground the response.
- Answer includes source information.
- A trace ID is generated for each request.
- Feedback can be recorded against the interaction.
- Telemetry can be inspected separately from the end-user UI.

## 10. Limitations

This is a portfolio/reference implementation rather than a production-ready SaaS application.

Current limitations include:

- Small local sample knowledge base.
- No user authentication or multi-tenancy.
- No persistent production database for feedback.
- No advanced evaluation framework for answer quality.
- Local vector/index storage in the reference setup.
- API/provider credentials are supplied by the operator through environment variables.

## 11. Potential Product Roadmap

### Phase 1 — MVP
- RAG chat
- Source display
- Trace IDs
- User feedback

### Phase 2 — AI Quality
- Retrieval quality metrics
- Groundedness/faithfulness evaluation
- Prompt/version tracking
- Feedback analytics
- Regression test dataset

### Phase 3 — Productionization
- Authentication and authorization
- Persistent vector database
- Persistent feedback store
- Rate limiting
- Cost and latency monitoring
- Error monitoring
- Model/provider abstraction

### Phase 4 — AI Product Platform
- Evaluation dashboards
- Experimentation/A-B testing
- Automated prompt/model comparison
- Human review workflows
- AI quality alerts

## 12. Portfolio Positioning

This project demonstrates the intersection of **AI product thinking and implementation**: a user-facing AI workflow is connected to retrieval, model generation, feedback, and observability. It is intentionally small enough to understand end-to-end while providing a foundation for discussing product decisions, metrics, trade-offs, and future roadmap opportunities.

## 13. Attribution

This project is adapted from the OpenObserve LangChain/LlamaIndex tracing demo. The original Apache 2.0 license and attribution are retained. Portfolio-specific work includes configuration handling, documentation, frontend/backend setup guidance, and developer-experience improvements.
