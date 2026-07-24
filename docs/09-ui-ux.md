# UI and UX

## Information architecture

```text
Public: landing page → sign in / create account
Authenticated: onboarding → personalized feed
                                  ├─ discover / search
                                  ├─ bookmarks
                                  ├─ article detail → perspectives → article chat
                                  ├─ global chat
                                  └─ profile and AI settings
```

## Core user flow

1. A new user creates an account.
2. Onboarding asks for topics, preferred explanation level, and preferred perspectives.
3. The personalized feed shows a small set of high-relevance stories with transparent “why this was recommended” signals.
4. The article detail page presents the source first, then the AI-generated summary and perspectives in separately labelled sections.
5. The user can save an article, refine their feed, or ask a question in the article chat.

## MVP screens

| Screen | Primary purpose | Required content |
| --- | --- | --- |
| Landing | Explain the product and invite signup | Brand, value proposition, product preview, sign-in CTA |
| Onboarding | Establish a useful starting profile | Topic picker, explanation level, preferred perspectives |
| Feed | Present personalized curation | Article cards, source, topic, relevance reason, save action |
| Article detail | Help users understand a story | Original source, summary, key takeaways, perspectives, chat entry point |
| Search / discover | Find topics and articles | Keyword and semantic search, topic filters, recent trends |
| Bookmarks | Return to saved reading | Saved articles, filters, remove action |
| Global chat | Explore curated knowledge | Question input, answer, cited source cards, scope indicator |
| Settings | Give users control | Provider, model, temperature, max tokens, data and personalization controls |

## Experience rules

- Every AI-created section must display an “AI-generated” label.
- Every summary and chat answer must link to its original article source or retrieved article cards.
- Perspectives are analytical lenses, not facts or advice.
- The product should remain readable and useful when AI processing is unavailable.
- Mobile layout must prioritize the feed, article reading, and chat over secondary controls.

## Next design deliverable

## Low-fidelity wireframes

### 1. Personalized feed

```text
┌──────────────────────────────────────────────────────────────────┐
│ CURA AI                         Discover  Saved  Chat  Settings   │
├──────────────────────────────────────────────────────────────────┤
│ Good morning, Shahin                                             │
│ Your briefing: 8 stories match LLMs · Agents · NVIDIA            │
├──────────────────────────────────────────────────────────────────┤
│ Topics  [AI agents] [LLMs] [OpenAI] [NVIDIA]       Filter ▾      │
├──────────────────────────────────────────────────────────────────┤
│  [Source]  6 min read                           ☆ Save            │
│  Article title with the most important news in one clear line     │
│  Short source excerpt that explains why the story matters.        │
│  Why recommended: Matches your interest in AI agents              │
├──────────────────────────────────────────────────────────────────┤
│  [Source]  4 min read                           ☆ Save            │
│  Next relevant article card ...                                  │
└──────────────────────────────────────────────────────────────────┘
```

The feed deliberately shows the source and recommendation rationale before AI analysis. A card opens the article detail page; saving must work without opening it.

### 2. Article detail

```text
┌──────────────────────────────────────────────────────────────────┐
│ ← Back to feed                         ☆ Save   Share source ↗   │
├──────────────────────────────────────────────────────────────────┤
│ [Official source] · 6 min read · AI agents                       │
│ Article title                                                     │
│ Source, publication time, and original-source link               │
├──────────────────────────────────────────────────────────────────┤
│ AI summary                                         AI-generated   │
│ A concise factual summary grounded in this article.               │
│ Key takeaways  • point one  • point two  • point three            │
├──────────────────────────────────────────────────────────────────┤
│ Perspectives                                      AI-generated   │
│ [Neutral] [Developer] [Student] [Investor] [Skeptical]            │
│ Selected perspective analysis, with a clear limitation note.      │
├──────────────────────────────────────────────────────────────────┤
│ Ask about this article                                            │
│ [What does this change for developers?                       ] ↗  │
│ Answers cite this article; expand to related stories if enabled. │
└──────────────────────────────────────────────────────────────────┘
```

The source article remains primary. AI sections are visibly separated and labelled; the perspective selector reveals one perspective at a time to avoid overwhelming readers.

### 3. AI settings

```text
┌──────────────────────────────────────────────────────────────────┐
│ Settings                                                          │
├──────────────────────────────────────────────────────────────────┤
│ Personalization                                                    │
│ Topics: [LLMs ×] [AI agents ×] [NVIDIA ×]          Edit topics    │
│ Explanation level:  ○ Beginner  ● Technical  ○ Balanced           │
├──────────────────────────────────────────────────────────────────┤
│ AI provider                                                        │
│ ● Gemini — hosted, default                                         │
│ ○ Ollama — local model; requires Ollama running on this device    │
│                                                                    │
│ Model                 [gemini model ▾]                            │
│ Temperature           [────●────] 0.4                             │
│ Maximum response size [2048 ▾]                                    │
│                                               [Save changes]      │
├──────────────────────────────────────────────────────────────────┤
│ Transparency                                                       │
│ ☑ Show recommendation reasons     ☑ Show AI labels                │
└──────────────────────────────────────────────────────────────────┘
```

Provider settings are user controls, but API keys are never shown or entered in the browser. The Ollama option stays disabled with an explanatory status when the local service is unavailable.

## Interaction decisions

- One primary action per screen: open a story from the feed, ask a question from an article, and save changes from settings.
- “Why recommended” is concise by default and may expand to reveal topic or behaviour signals.
- Chat defaults to the current article; widening to related articles is an explicit user action.
- AI settings use safe defaults: a low temperature and bounded response size.
