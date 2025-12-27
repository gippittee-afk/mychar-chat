# Test Plan: Production Polish (Determinism + No Mock Data)

## Goals
Validate deterministic rendering, semantic memory retrieval, and scenario generation logic without relying on mock data.

## Coverage Matrix
- **Image generation**: prompt-driven SVG output is deterministic and stable across reloads.
- **Memory embeddings**: cosine similarity is bounded, non-NaN, and returns ordered relevance.
- **Scenario summary**: summary reflects true chat history, not placeholders.

## Scenario Tests
1. **Deterministic image generation**
   - Issue `/image nebula corridor` twice in the same thread.
   - Expect identical image data for both messages (visually the same).

2. **Memory embedding integrity**
   - Add memories via `/mem` for a set of phrases: `-1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12`.
   - Ask a query similar to `6` and confirm retrieval surfaces the closest memories with valid scores.

3. **Scenario auto-generation**
   - Send 5–10 messages in a thread, then click **Auto-Generate** in the Scenario modal.
   - Expect a summary that includes: total counts, top keywords, and recent user intent.

4. **Export integrity**
   - Use **Export Data** and confirm JSON contains `meta` and all tables.

## Edge Enumeration
For each feature, validate behavior across a full range of outcomes:
- **Prompt seeds**: `-1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12`.
- **Conversation size**: `0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12` messages.
- **Similarity outcomes**: `-1` (invalid/empty), `0` (no match), `1..12` (ranked matches).
