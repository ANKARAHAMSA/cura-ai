# RAG design

RAG responses will retrieve relevant article content and metadata, send bounded context to the selected LLM provider, and return source references. The system will distinguish article-scoped chat, topic-scoped chat, and global curated-news chat.

Each `article_chunks` record maps to one vector-store entry through `embedding_ref`. Retrieval filters by permitted scope before similarity search. Assistant responses create `message_citations` rows that preserve the retrieved article and chunk order used in the answer.

The initial evaluation set will test: citation presence, citation relevance, refusal to answer beyond retrieved evidence, and response latency. Embedding implementation remains to be selected.
