---
last-updated: 2026-07-01
target-stack: Next.js 13/14 (App Router), React 18
status: Accepted
---

# ADR-001: SSR vs SPA

## Status

Accepted

---

## Context

The application needed to support multiple frontend modules with different requirements:

- authenticated workflows
- data-heavy screens
- integration with backend services

At the same time, the goal was to keep a consistent approach across the platform.

---

## Decision

Use SSR (via Next.js) as the default rendering strategy.

---

## Alternatives

### SPA (client-side rendering only)

Pros:

- simpler setup
- easier debugging
- fewer moving parts

Cons:

- more logic pushed to the client
- less control over data loading
- harder to standardize across multiple applications

---

### Mixed approach (SSR + SPA depending on page)

Pros:

- flexibility
- can optimize specific use cases

Cons:

- inconsistent patterns across the codebase
- harder for teams to follow a single approach

---

## Rationale

SSR was chosen mainly for consistency.

Having a single model for:

- rendering
- data loading
- integration with backend services

was considered more important than optimizing individual pages.

It also allowed introducing a server-side layer (Route Handlers / proxy) without adding extra infrastructure.

---

## Trade-offs

- more complexity compared to SPA
- need to manage server/client boundaries
- debugging can be less straightforward

These were considered acceptable given the scale of the system.

---

## Consequences

Positive:

- consistent structure across applications
- easier integration with authentication and APIs
- better control over request flow

Negative:

- higher learning curve
- more complex runtime model

---

## Notes

This decision is tied to the platform approach.

In smaller projects, a SPA would likely be a simpler and valid choice.

---

## Field note: how "SSR as default" looked in practice

A real-world project following this ADR's decision had roughly 85% of its
components marked `"use client"` (a rough count: ~705 out of ~830 non-test
`.tsx` files under the app and feature directories). Route-level `page.tsx`
files were consistently thin server shells: metadata, redirects, and rendering
mode directives, immediately delegating to a client component that owned data
fetching, permissions, and state:

```tsx
// app/(main)/dashboard/page.tsx: Server Component
export const dynamic = "force-dynamic";

export async function generateMetadata(): Promise<Metadata> {
  const t = await getTranslations("dashboard");
  return { title: t("pageTitle") };
}

export default function DashboardPage() {
  return <Dashboard />;
}
```

```tsx
// features/dashboard/dashboard.tsx: Client Component
"use client";

export function Dashboard() {
  // data fetching, permission checks and state all happen here,
  // client-side, after hydration
}
```

This is not necessarily a violation of the ADR: SSR is still the entry point,
and the "server-side layer (Route Handlers / proxy)" mentioned in the
Rationale is real (see the BFF pattern in
[next-platform.md](../case-studies/next-platform.md)). But in practice,
"SSR as default rendering strategy" mostly bought consistent routing,
metadata, and an authenticated request boundary — not server-rendered data
for most screens. Authenticated, data-heavy, highly interactive screens (the
majority of this codebase) ended up client-rendered after an initial thin
server shell. Teams evaluating this ADR should not assume "SSR by default"
means "most UI is server-rendered" — on this evidence, it did not.