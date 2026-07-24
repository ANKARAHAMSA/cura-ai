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

Low-fidelity wireframes for the feed, article detail, and settings screens.
