---
name: typescript-ecosystem
description: TypeScript configuration, type patterns, and ecosystem decisions. Use when setting up a new project's TypeScript config, choosing between strictness levels, designing type-safe APIs, or reviewing type coverage across the codebase.
---

# TypeScript Ecosystem

Make TypeScript work for your team, not against it.

## Configuration Strategy

Start with `strict: true` on every new project. The strict flags that matter most:

| Flag | What it prevents | Cost |
|------|------------------|------|
| `strictNullChecks` | `null`/`undefined` access | Low |
| `noUncheckedIndexedAccess` | Unsafe object access | Medium |
| `strictFunctionTypes` | Unsafe function variance | Low |
| `exactOptionalPropertyTypes` | Accidental `undefined` writes | Medium |
| `noUnusedLocals` | Dead code | Low |

**For existing codebases**: enable strict mode, fix the errors, and never look back. If the codebase is too large, enable flags one at a time per directory using `overrides` in tsconfig.

## Type Patterns

### Prefer `type` over `interface` for data

Use `interface` only for public API contracts that may be extended. Use `type` for everything else — union types, intersection types, tuples, and mapped types are impossible with `interface`.

```ts
type User = {
  id: string
  email: string
  role: "admin" | "user"  // union — impossible with interface
}
```

### Branded Types for Primitive Safety

```ts
type Brand<T, B> = T & { __brand: B }

type UserId = Brand<string, "UserId">
type PostId = Brand<string, "PostId">

function getUser(id: UserId) { /* ... */ }

const userId = "abc" as UserId
const postId = "xyz" as PostId

getUser(userId)   // OK
getUser(postId)   // Type error — correct!
```

### Discriminated Unions for State Machines

```ts
type RequestState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; error: Error }
```

Every state is fully represented in the type system — impossible states are unrepresentable.

## Dependency Decisions

| Library | When to use | When to avoid |
|---------|-------------|---------------|
| Zod | Runtime validation of external data (API responses, form data) | Internal-only types (use `satisfies`) |
| tRPC | End-to-end type safety, no API client code | Public APIs consumed by non-TS clients |
| Prisma | Type-safe database access, migrations | Complex queries beyond ORM capabilities (use raw SQL) |
| Valibot | Smaller bundle, modular | Need Zod's ecosystem (infer, describe, superRefine) |

## What Nobody Mentions

TypeScript is a productivity tool, not a purity test. A `@ts-expect-error` with a documented reason is better than a 50-line type gymnastic that nobody understands. The goal is shipping with confidence, not 100% type coverage.

## References

See `references/tsconfig-templates.md` for recommended tsconfig setups by project type.
