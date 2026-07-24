# System architecture

## MVP architecture

```text
Next.js web app
       │ HTTPS
FastAPI API
       ├─ authentication and profiles
       ├─ article, bookmark, search, and chat services
       ├─ provider-neutral LLM service ── Gemini / Ollama
       ├─ ingestion and processing workers
       │      ├─ RSS + approved-source collector
       │      ├─ duplicate detection
       │      ├─ tagging and summarization
       │      └─ embedding generation
       ├─ PostgreSQL (accounts, articles, preferences, events)
       └─ vector store (retrieval embeddings)
```

## Provider-neutral AI contract

Business services call a shared interface rather than Gemini or Ollama directly. Each provider adapter supports generation and health checks; embeddings are a separate service because they may use a local model independently of the selected chat provider.

```text
Analysis / Chat Service → LLMProvider interface → GeminiProvider | OllamaProvider
Retrieval Service       → EmbeddingProvider interface → local embedding model
```

## Data flow

1. A scheduled worker reads approved sources and stores normalized article metadata.
2. The processing worker detects duplicates, tags articles, generates a summary and perspectives, then creates embeddings.
3. The feed service combines explicit preferences, recent content, and behaviour signals to rank articles.
4. Chat retrieves relevant chunks, asks the selected LLM with bounded context, and returns citations.

## Boundaries

The frontend never calls LLM providers or databases directly. API keys remain server-side. Long-running ingestion and processing are kept out of web-request paths.
