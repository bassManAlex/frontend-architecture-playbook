# ADR-002 — Monorepo

## Status

Accepted

---

## Context

The platform includes multiple frontend applications with shared parts:

- UI components
- utilities
- common logic

Different teams are working on overlapping areas.

Keeping everything aligned across separate repositories was becoming difficult.

---

## Decision

Use a monorepo structure with shared packages.

---

## Alternatives

### Multiple repositories (polyrepo)

Pros:

- clear separation between projects
- simpler repository structure

Cons:

- duplication of shared code
- difficult to keep versions aligned
- changes need to be replicated manually

---

### Monorepo without clear structure

Pros:

- easy access to all code
- simple setup

Cons:

- risk of coupling everything together
- hard to scale without conventions

---

## Rationale

The main driver was reducing duplication.

Having a single place for:

- shared components
- common logic

makes it easier to keep things consistent.

It also simplifies refactoring across applications.

---

## Trade-offs

- requires discipline to avoid tight coupling
- CI/CD becomes more complex
- changes in shared code can impact multiple areas

---

## Consequences

Positive:

- shared code is easier to maintain
- less duplication
- easier cross-project changes

Negative:

- coordination between teams is required
- ownership of shared code must be defined

---

## Notes

This setup works because:

- the platform is shared across multiple tenants
- consistency is more important than isolation

For completely independent projects, separate repositories would still make sense.