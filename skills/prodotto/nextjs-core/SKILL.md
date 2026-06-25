---
name: nextjs-core
description: Next.js App Router development, Server Components, data fetching patterns, caching, routing, and performance optimization. Use when building or reviewing Next.js applications — new projects, feature implementation, page structure, layout design, data layer architecture, middleware, and build configuration.
---

# Next.js Core

Authoritative patterns for Next.js App Router applications.

## Rendering & Data Fetching

Prefer Server Components by default. Push `"use client"` boundaries as deep as possible.

```tsx
// Server Component — default, no directive needed
async function Page() {
  const data = await fetchData()
  return <UI data={data} />
}
```

Use `"use cache"` (Next.js 16+) for caching with fine-grained invalidation:

```tsx
async function getCachedData() {
  "use cache"
  cacheLife("hours")
  cacheTag("my-data")
  const res = await fetch("https://api.example.com/data")
  return res.json()
}
```

**Built-in `cacheLife` profiles**: `seconds` (30s/1s/1m), `minutes` (5m/1m/1h), `hours` (5m/1h/1d), `days` (5m/1d/1w), `weeks` (5m/1w/30d), `max` (5m/30d/1y).

**Deprecated in v16**: `export const revalidate` → use `cacheLife()`; `export const dynamic` → use `"use cache"` + `<Suspense>`.

## Route & Layout Patterns

| File | Purpose |
|------|---------|
| `layout.tsx` | Shared chrome (nav, sidebar, footer) — state persists across navigations |
| `template.tsx` | Resets state on navigation (analytics, animations) |
| `page.tsx` | Content composition only — no boilerplate, logic, or styling |
| Route groups `(name)/` | Logical grouping without URL side effects |

pages should be thin — composition of sections, not implementation.

## Async APIs (Next.js 16)

`params` and `searchParams` are Promises — must be awaited:

```tsx
type Props = { params: Promise<{ slug: string }> }

export default async function Page({ params }: Props) {
  const { slug } = await params
  // ...
}
```

## Server Actions

For mutations only, not data fetching. Validate input at the boundary:

```tsx
"use server"
import { z } from "zod"

const schema = z.object({ title: z.string().min(1) })

export async function createPost(formData: FormData) {
  const parsed = schema.parse({ title: formData.get("title") })
  // mutate + revalidateTag / updateTag
}
```

Use `updateTag()` inside Server Actions for immediate read-your-writes; use `revalidateTag()` in Route Handlers and webhooks.

## Metadata & SEO

Export `viewport` separately from `metadata`:

```tsx
export const viewport: Viewport = {
  width: "device-width",
  initialScale: 1,
  themeColor: [
    { media: "(prefers-color-scheme: light)", color: "#ffffff" },
  ],
}

export const metadata: Metadata = {
  metadataBase: new URL("https://example.com"),
  title: { default: "Site", template: "%s | Site" },
  description: "…",
  openGraph: { images: "/og.png" },
}
```

Prefer file conventions (`opengraph-image.*`, `icon.*`, `sitemap.ts`, `robots.ts`) over metadata object overrides — they auto-emit tags and are statically optimized.

## References

See `references/caching.md` for advanced `"use cache"` patterns, cache tags, and revalidation strategies.
See `references/deployment.md` for build configuration, output settings, and platform-specific tuning.
