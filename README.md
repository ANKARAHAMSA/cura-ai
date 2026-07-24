# CURA AI

> Curated Intelligence. Personalized Insights.

CURA AI is an AI-powered personalized news intelligence platform. It will curate trustworthy AI and technology news, adapt to each user's interests, present clearly labelled perspectives, and support grounded conversations with articles and topics.

## Planned capabilities

- Personalized news feed and recommendations
- AI summaries, key takeaways, and multi-perspective analysis
- Article and cross-news RAG chat
- Gemini as the default provider, with optional local Ollama models
- Semantic search, bookmarks, reading history, and user preferences

## Planned stack

- Frontend: Next.js, TypeScript, Tailwind CSS
- Backend: FastAPI, Python, PostgreSQL
- AI: Gemini and Ollama through a provider-neutral service layer
- Retrieval: local embeddings and a vector database

## Repository layout

```text
frontend/  # Next.js application
backend/   # FastAPI application
docs/      # Product and engineering decisions
docker/    # Container configuration
scripts/   # Developer and maintenance scripts
```

## Status

Planning and project foundation. See [the roadmap](docs/10-development-roadmap.md).

## Documentation

The `docs/` directory records the product vision, requirements, architecture, data design, API design, retrieval design, and roadmap before implementation begins.

## License

License selection is pending.
