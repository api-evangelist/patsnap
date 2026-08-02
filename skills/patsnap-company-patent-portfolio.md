---
name: Analyze a company's patent portfolio
description: Find all patents assigned to a company, retrieve concise bibliographic records, and resolve their patent families to understand global coverage.
api: openapi/patsnap-openapi-original.json
operations:
  - p004PatentCompanySearch
  - p011BasicPatentDataSimpleBibliography
  - p014BasicPatentDataPatentFamily
---

# Analyze a company's patent portfolio

Use the Patsnap Open Platform API (base `https://connect.patsnap.com`) to profile an assignee's IP holdings.

## Auth
`Authorization: Bearer sk-...` on every request.

## Steps
1. **Find the company's patents** — `POST /search/patent/company-search-patent` (`p004PatentCompanySearch`) with the company / assignee name to retrieve its patents. Page with limit/offset and collect patent ids.
2. **Get concise records** — for each hit, `GET /basic-patent-data/simple-bibliography` (`p011BasicPatentDataSimpleBibliography`) for a lightweight title/assignee/date record suitable for tabulating a portfolio.
3. **Resolve families** — `GET /basic-patent-data/patent-family` (`p014BasicPatentDataPatentFamily`) per patent to group applications into families and gauge multi-jurisdiction coverage.

## Conventions
- One assignee's portfolio can be large — page deliberately and cache patent ids.
- Field-type aggregate analysis is capped (≤ 500 one-dimensional buckets).
- Watch for `401`/`403` (auth/permission) and platform rate-limit codes in the `67200000-67200101` range (`errors/patsnap-error-codes.yml`).
- All three operations are read-only and idempotent.
