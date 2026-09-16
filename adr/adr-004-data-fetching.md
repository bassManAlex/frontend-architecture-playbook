---
last-updated: 2026-07-01
target-stack: Next.js 13/14 (App Router), React 18, React Query
status: Accepted
---

# ADR-004: Data Fetching Strategy

> **Stack note.** This document is a historical record of a completed engagement built on Next.js 13/14 (App Router), React 18, React Query. It is not a description of current practice; current work uses Next.js 16 and React 19.

## Status

Accepted

---

## Context

The platform serves multiple teams building independent applications on the same
foundation ([ADR-002](adr-002-modorepo-choice.md)). Each application needs to fetch
server data both for the initial page render (via SSR) and for subsequent client-side
interactions (refetching, mutations, pagination, polling).

Without a shared approach, different teams tended to implement loading, error and
retry handling in their own way, which worked against the consistency goal already
established for rendering ([ADR-001](adr-001-ssr-vs-spa.md)) and state management
([ADR-003](adr-003-state-management.md)).

---

## Decision

Use React Query as the standard data fetching layer for server state, across both
the initial SSR-rendered data and subsequent client-side interactions.

Server-side: data is prefetched during SSR and passed to the client via
dehydrate/hydrate, so the client starts from an already-populated cache instead of
re-fetching on mount.

Client-side: React Query owns refetching, caching, invalidation and mutations for
any data that changes after the initial render.

---

## Alternatives

### Native fetch in Server Components only (no client-side library)

Pros:

- fewer dependencies
- simpler mental model for server-rendered data

Cons:

- no standard pattern for client-side refetching, caching or mutations
- each team would need to build its own loading/error/retry handling

---

### SWR

Pros:

- similar feature set to React Query
- lighter footprint

Cons:

- not evaluated in depth; React Query was already the incumbent choice across teams,
  so switching was not justified without a concrete gap

---

### RTK Query

Pros:

- integrates with Redux, which is already used for UI-related client state
  ([ADR-003](adr-003-state-management.md))

Cons:

- ties server-state caching to the Redux store, which ADR-003 deliberately avoided
  (server data is handled outside global stores)

---

## Rationale

The primary driver was standardization: giving every team the same pattern for
loading, error and retry state, rather than optimizing for a specific performance
characteristic of one library over another.

Because SSR is already the default rendering strategy, prefetching data on the
server and hydrating React Query's cache on the client avoids a duplicate fetch on
first load, while still giving teams a single, consistent API for everything that
happens after hydration.

---

## Trade-offs

- adds a dependency and a caching layer on top of what Server Components already
  provide natively
- prefetch/dehydrate/hydrate wiring must be set up consistently across applications,
  or the benefit of a single pattern is lost
- teams need to understand both the SSR data flow and React Query's client-side
  cache lifecycle, which is more conceptual surface than either alone

---

## Consequences

Positive:

- consistent loading/error/retry handling across all applications
- no duplicate fetch between server render and client hydration
- clear ownership: React Query for server state, Context/Redux for everything else
  ([ADR-003](adr-003-state-management.md))

Negative:

- additional setup cost per application (prefetch/dehydrate wiring)
- another concept teams need to onboard onto, on top of SSR itself

---

## Notes

This decision assumes React Query is used consistently across applications; a team
adopting it only partially (e.g. client-side only, without prefetch/hydrate) would
not get the full benefit described here.

Open question not yet settled in this document: how cache invalidation is
coordinated across tenants when the same shared data changes (see the accessibility,
performance and security documents for related open questions still pending
verification).

---

## Field note: what happens when this ADR is not followed

See the field note in [ADR-003](adr-003-state-management.md#field-note-where-this-decision-drifted-in-practice)
for a concrete (anonymized) example from a real-world project where server data
was fetched and cached by hand inside Redux slices instead of through this layer.
The practical cost observed was not a performance problem but a consistency one:
each slice re-implemented its own `idle/loading/succeeded/failed` status, its own
error field, and its own guard against redundant refetching. That is exactly the
boilerplate this ADR's "standardization" rationale is meant to remove. A codebase
can adopt Redux Toolkit for UI state (per ADR-003) and still drift into this if
"server state goes through React Query" is not enforced consistently for every
new slice a team adds.
