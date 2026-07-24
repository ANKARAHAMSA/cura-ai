# CURA AI continuation handoff

## Product

**CURA AI — Curated Intelligence. Personalized Insights.**

CURA AI is a multi-user AI and technology news intelligence platform. It curates trusted sources, personalizes each feed, creates clearly labelled multi-perspective article analysis, and lets users chat with curated content using RAG.

## Repository

- GitHub: `https://github.com/ANKARAHAMSA/cura-ai`
- Local folder: `/Users/shahinhamza/Documents/Codex/cura-ai`
- Main branch is the active branch and is pushed after each milestone.
- Commit style: Conventional Commits, for example `docs: define CURA AI API contract`.

## Decisions already made

### Stack

- Frontend: Next.js, TypeScript, Tailwind CSS.
- Backend: FastAPI, Python, SQLAlchemy, Alembic.
- Relational data: PostgreSQL.
- Retrieval: a vector store plus local embeddings (specific embedding implementation still to be selected).
- LLMs: Gemini as the hosted default and optional local Ollama support.
- Design rule: application services use provider-neutral LLM and embedding interfaces; do not call Gemini or Ollama directly from UI code.

### MVP scope

- Accounts and individual user preferences.
- Curated AI and technology news via approved sources / RSS.
- Personalized feed, bookmarks, reading events, and semantic search.
- Article summaries, key takeaways, and labelled perspectives: neutral, developer, student, investor, economist, optimistic, and skeptical.
- RAG chat scoped to an article, topic, or the global curated-news collection, with citations.
- Settings for provider, model, temperature, and response length.

Not part of MVP: weekly newsletters, timelines, knowledge graphs, voice, mobile app, social-media ingestion, and market predictions.

### UX decisions

- The feed first shows original source and a short “why recommended” reason.
- Article pages keep source content primary; all AI sections are visibly labelled.
- The article chat starts article-scoped. Expanding to related content is an explicit action.
- Settings must never collect or expose server API keys. Ollama settings only control safe local model parameters.

## Documents completed

| Document | Contents |
| --- | --- |
| `docs/00-project-vision.md` | Mission, product principles |
| `docs/01-product-requirements.md` | MVP scope, priorities, acceptance criteria |
| `docs/02-user-personas.md` | Student, developer, AI enthusiast, investor/analyst personas |
| `docs/03-user-stories.md` | Initial user stories |
| `docs/04-system-architecture.md` | Next.js/FastAPI/workers/LLM provider architecture |
| `docs/05-database-design.md` | ERD, tables, constraints, privacy rules |
| `docs/06-api-design.md` | `/api/v1` REST API contract |
| `docs/07-rag-design.md` | Retrieval and citation approach |
| `docs/08-recommendation-engine.md` | Feed signals and explainable ranking |
| `docs/09-ui-ux.md` | Information architecture and low-fidelity wireframes |
| `docs/10-development-roadmap.md` | Live progress tracker |

## Current state

Planning, UI/UX, database design, and API design are complete. There is no application code yet. The repository currently contains project structure and documentation only.

## Immediate next task

Build the backend foundation on a `feature/backend-foundation` branch.

Recommended first slice:

1. Create the FastAPI app under `backend/` with a `src/` package structure.
2. Add environment configuration with `.env.example`; never commit secrets.
3. Add Docker Compose for PostgreSQL and the backend local development environment.
4. Add SQLAlchemy database connection, Alembic, and a basic `users` model.
5. Add `GET /api/v1/health` and `GET /api/v1/ready`.
6. Add formatting, linting, and a minimal test setup.
7. Document setup in `README.md` and commit in small, meaningful commits.

## Suggested prompt for another assistant

```text
I am continuing the CURA AI project at https://github.com/ANKARAHAMSA/cura-ai.

Read README.md and every file in docs/ before making changes. CURA AI is a multi-user AI/technology news intelligence platform with a Next.js frontend and FastAPI backend. Gemini is the default hosted LLM, Ollama is optional local support, and the backend must remain provider-neutral.

Planning is complete. Implement the next milestone: backend foundation. Create a feature/backend-foundation branch; scaffold FastAPI under backend/ with environment configuration, Docker Compose PostgreSQL, SQLAlchemy, Alembic, basic user model, /api/v1/health and /api/v1/ready endpoints, formatting/linting, and one minimal health test. Do not implement authentication or AI features yet. Keep docs and README accurate. Use Conventional Commits, test before committing, and push commits to GitHub.
```
