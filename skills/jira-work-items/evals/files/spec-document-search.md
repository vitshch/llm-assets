# Document Search — Spec (draft 2)

## Background
Consumers of the Documents platform can list buckets and fetch a document if they already know its ID.
They cannot find documents by content or metadata. Support gets ~15 tickets a week asking "where is
the invoice for customer X".

## Requirements
1. A consumer can search documents within one bucket by free text (file name and extracted text).
2. A consumer can filter results by metadata: `createdBy`, `createdAt` range, `contentType`.
3. Results are paginated (default 20, max 100) and sorted by relevance, or by `createdAt` on request.
4. Only documents in buckets the caller may access are returned. Roles `ADMIN`, `CONSUMER`, `PROVIDER`
   may search; scope `documents:search`.
5. Search must reflect a newly uploaded document within 60 seconds.
6. Web UI gets a search box on the bucket page with filter chips and a result list.

## Open points
- Full-text search over PDF/scanned content: we don't extract text from scans today. Do we need OCR
  for v1? Product says "nice to have", engineering doesn't know the cost.
- Which search engine? Postgres FTS is already available; OpenSearch would need a new cluster and
  the platform team has not confirmed capacity.
- Cross-bucket search is explicitly NOT in v1, but the API shape should not block it later.

## Non-functional
- p95 latency under 500 ms for buckets up to 1M documents.
- No new PII stored outside the existing document store.
