---
name: api-design
description: Design APIs that are consistent, discoverable, and resilient. Use when designing new API endpoints, reviewing API contracts, choosing between REST and GraphQL, defining error responses, or planning versioning strategy.
---

# API Design

Practical patterns for APIs that survive production.

## REST-First, GraphQL When Needed

Start with REST. It is simple, cacheable, and tooling-rich. Reach for GraphQL when:

- Multiple clients need different data shapes (mobile vs web vs 3rd party)
- Over-fetching is a measurable performance problem (not a theoretical one)
- You need real-time subscriptions

**GraphQL is not a replacement for REST** — it solves a specific set of problems and introduces its own (caching, query cost analysis, resolver N+1).

## URL Structure

```
POST   /api/v1/resource          # C-create
GET    /api/v1/resource          # R-list
GET    /api/v1/resource/:id      # R-read
PATCH  /api/v1/resource/:id      # U-update (partial)
DELETE /api/v1/resource/:id      # D-delete
```

Rules:
- Plural nouns for collections (`/users`, not `/user`)
- Version from day one (`/api/v1/`) — even if you never break it, the option costs nothing
- Nest only when the child resource is meaningless without the parent (`/users/:id/posts`, not `/posts?userId=:id`)
- Actions that do not map to CRUD get a verb (`/api/v1/resource/:id/archive`, not `PATCH /api/v1/resource/:id { "status": "archived" }`)

## Error Responses

Every error must have this shape:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Title must be at least 3 characters",
    "details": [
      { "field": "title", "code": "too_short", "min": 3 }
    ]
  }
}
```

HTTP status codes are necessary but not sufficient. The `code` field is a stable machine-readable identifier that clients can switch on. Never change a `code` without versioning. The `message` is for developers, not end users.

## Pagination

Cursor-based pagination is the default. Page-based (offset/limit) for admin panels and internal tools only:

```json
// Request
GET /api/v1/posts?cursor=eyJpZCI6IjEyMyJ9&limit=20

// Response
{
  "data": [...],
  "pagination": {
    "nextCursor": "eyJpZCI6IjE0MyJ9",
    "hasMore": true
  }
}
```

## Versioning

- **URL versioning** (`/api/v1/`) — simple, explicit, permanent
- **Header versioning** (`Accept: application/vnd.myapp.v2+json`) — cleaner URLs, harder to debug
- **Never** remove a field without a deprecation period. Add `deprecated` to the schema, log usage, remove after N months

## References
See `references/error-codes.md` for a catalog of standard error codes and their meanings.

See `references/rest-vs-graphql.md` for a detailed comparison and decision tree.
