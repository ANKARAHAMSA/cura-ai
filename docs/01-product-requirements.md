# Product requirements

## Problem

News platforms often overwhelm users with generic feeds and provide little help understanding relevance, context, or differing viewpoints.

## MVP goals

- Multi-user accounts and individual preferences.
- Trusted AI and technology news collection with deduplication.
- Personalized feed, reading history, bookmarks, and semantic search.
- AI summary, key takeaways, and labelled neutral, developer, student, investor, economist, optimistic, and skeptical perspectives.
- RAG chat scoped to an article, a topic, or the curated knowledge base.
- Gemini by default, with optional Ollama configuration for local models.

## MVP boundaries

The first release focuses only on AI and technology news. It starts with a small, curated set of RSS and official company-blog sources. Automated email digests, knowledge graphs, timelines, voice, mobile applications, social-media ingestion, and financial predictions are deliberately outside the MVP.

## Feature priority

| Priority | Included capability |
| --- | --- |
| Must have | Authentication, source ingestion, article feed, article detail, bookmarks, topic preferences, Gemini summaries, labelled perspectives, article chat, provider settings |
| Should have | Semantic search, global curated-news chat, reading-history signals, basic recommendations, Ollama support |
| Later | Weekly digest, timeline, knowledge graph, learning quizzes, notifications, mobile app |

## Non-functional requirements

- Responsive interface, secure authentication, modular code, grounded answers with citations, and provider-independent AI services.

## Acceptance criteria for the MVP

1. A new user can register, select topics, and receive a relevant feed.
2. A user can open an article, see its source, AI summary, and clearly-labelled perspectives.
3. Article chat answers only from retrieved article content and identifies its sources.
4. A user can bookmark articles and edit personal preferences.
5. An administrator or scheduled worker can ingest new articles without creating duplicates.
6. Gemini works as the initial hosted provider; Ollama can be selected when it is running locally.
