# System architecture

The planned architecture is a Next.js frontend, FastAPI backend, PostgreSQL metadata store, retrieval store, background news collection and AI-processing workers, and a provider-neutral LLM service layer.

```text
Next.js → FastAPI → application services → PostgreSQL / vector store
                         ├─ news collection workers
                         ├─ Gemini provider
                         └─ Ollama provider
```
