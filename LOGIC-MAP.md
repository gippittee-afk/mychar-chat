# Logic Map: Production Polish (No Mock Data + Deterministic Outputs)

## Goal
Eliminate mock content by replacing placeholder data generation with deterministic, testable logic while preserving the single-file architecture and UI behavior.

## Logic Chain (Steps 1–3 Bundled)
1. **Replace mock images with deterministic, prompt-derived rendering**  
   - **Why**: External placeholder images are non-deterministic and not representative of the prompt.  
   - **How**: Use a seeded PRNG derived from a stable hash of the prompt to generate an SVG scene.  
   - **Mathematical correctness**: The hash → seed mapping is deterministic. The seeded generator yields reproducible floating-point sequences, making the same prompt produce the same image.  

2. **Replace dummy embeddings with real embeddings from the vector engine**  
   - **Why**: A fixed array does not preserve semantic distance and breaks cosine similarity invariants.  
   - **How**: Use the same feature-hashing + normalization pipeline as runtime embeddings to seed default memory.  
   - **Mathematical correctness**:  
     - Each vector is normalized to unit length (or safe-zero fallback), satisfying cosine similarity requirements.  
     - Cosine similarity now has a bounded denominator with explicit zero-division handling.  

3. **Replace placeholder summaries with computed, evidence-driven summaries**  
   - **Why**: Static strings do not reflect actual message content and cannot guide scenario generation.  
   - **How**: Extract token frequencies, compute top themes, capture last user/assistant messages, and infer tone based on conversation signals.  
   - **Mathematical correctness**: Term frequency is calculated with explicit normalization (stop-word filtering, length constraints) to avoid bias toward filler tokens.

## Verification
- **Determinism**: The same prompt or avatar seed yields the same SVG output across sessions.
- **Semantic similarity**: Stored memories now share the same embedding pipeline as queries, preserving cosine ranking.
- **Scenario coherence**: Auto-generated summaries are derived from real message history rather than placeholders.
