---
name: Run a semantic prior-art / similarity search
description: Use semantic and similarity search over the Patsnap corpus to surface prior art for an idea or reference patent, then compare claim language.
api: openapi/patsnap-openapi-original.json
operations:
  - p008PatentSemanticSearch
  - p007PatentSimilarSearch
  - p018BasicPatentDataClaimData
---

# Semantic prior-art / similarity search

Use the Patsnap Open Platform API (base `https://connect.patsnap.com`) to find prior art by meaning, not just keywords.

## Auth
`Authorization: Bearer sk-...` on every request.

## Steps
1. **Semantic search from a description** — `POST /search/patent/semantic-search-patent` (`p008PatentSemanticSearch`) with a natural-language description of the invention to retrieve semantically-ranked patents.
2. **Find similar to a reference patent** — when you already have a reference publication number, `POST /search/patent/similar-search-patent` (`p007PatentSimilarSearch`) to pull the most similar patents.
3. **Compare claims** — for the strongest candidates, `GET /basic-patent-data/claim-data` (`p018BasicPatentDataClaimData`) to read independent/dependent claims and assess overlap with your target.

## Conventions
- Semantic queries accept free text; keep the description focused for best ranking.
- Read operations are idempotent; page results with limit/offset.
- Errors follow the numeric platform/business code envelope (`errors/patsnap-error-codes.yml`); mind QPS and daily limits (`lifecycle/patsnap-lifecycle.yml`).
