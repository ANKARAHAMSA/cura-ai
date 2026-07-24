# Recommendation engine

Recommendations will begin with explicit interests and recency. Later signals may include reading time, clicks, bookmarks, searches, and feedback. The system must allow users to inspect and adjust their preferences.

Each generated candidate is stored in `recommendations` with its ranking score, a concise user-facing reason, and an `algorithm_version`. This makes the personalized feed explainable and lets future ranking changes be evaluated safely.
