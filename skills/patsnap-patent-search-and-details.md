---
name: Search patents and retrieve bibliography and legal status
description: Query the Patsnap patent corpus, count and page results, then pull full bibliographic and legal-status data for the patents you find.
api: openapi/patsnap-openapi-original.json
operations:
  - p001PatentQuerySearchCount
  - p002PatentQuerySearch
  - p012BasicPatentDataBibliography
  - p013BasicPatentDataLegalStatus
---

# Search patents and retrieve details

Use the Patsnap Open Platform API (base `https://connect.patsnap.com`) to search patents and enrich the hits.

## Auth
Every request carries your API key as a bearer token: `Authorization: Bearer sk-...`. Keys are provisioned in the Open Platform console. Never place the key in a query string in production.

## Steps
1. **Size the result set** — `POST /search/patent/query-search-count` (`p001PatentQuerySearchCount`) with your query text/expression to get the total match count before paging. Respect field-analysis caps (one-dimensional field analysis ≤ 500).
2. **Retrieve the matches** — `POST /search/patent/query-search-patent` (`p002PatentQuerySearch`), paging with limit/offset. Collect each patent identifier (patent_id / publication number) from the response.
3. **Pull bibliography** — for each patent, `GET /basic-patent-data/bibliography` (`p012BasicPatentDataBibliography`) keyed by the patent id to get title, assignee, inventors, dates and classifications.
4. **Check legal status** — `GET /basic-patent-data/legal-status` (`p013BasicPatentDataLegalStatus`) to confirm whether each patent is active, lapsed, or granted.

## Conventions
- Pagination is limit/offset (see `conventions/patsnap-conventions.yml`).
- Errors return a numeric platform/business code + message; `401`/`403` mean key/permission problems (see `errors/patsnap-error-codes.yml`).
- Search/read operations are idempotent; no idempotency key is required.
- Respect per-account QPS and daily usage limits (`lifecycle/patsnap-lifecycle.yml`).
