# ADR-001 — SSR vs SPA

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

It also allowed introducing a server-side layer (API routes / proxy) without adding extra infrastructure.

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