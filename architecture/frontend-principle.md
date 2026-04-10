# Frontend Principles

This document collects a set of practical guidelines used when working on frontend systems at scale.

They are not strict rules, but recurring patterns that proved useful across different projects.

---

## 1. Prefer structure over speed (in the long run)

Quick solutions tend to accumulate and become harder to manage.

When the same pattern appears more than once, it usually means it should be structured properly.

---

## 2. Consistency matters more than local optimization

Different teams solving the same problem in different ways creates friction.

Even if a solution is not perfect, consistency across the codebase usually pays off.

---

## 3. Avoid mixing concerns

Try to keep clear boundaries between:

- domain logic
- shared utilities
- UI components

Once these start to overlap, changes become harder and side effects increase.

---

## 4. Keep features self-contained

Each feature should be understandable on its own.

A developer should be able to work on a feature without needing to navigate the entire project.

---

## 5. Don’t abstract too early

Abstractions are useful, but only when there is something real to abstract.

If something is used only once, it probably doesn’t belong in a shared layer yet.

---

## 6. Shared code needs ownership

Reusable components and utilities don’t maintain themselves.

Without clear ownership, they tend to degrade over time.

---

## 7. Treat the frontend as a system, not just pages

As applications grow, the frontend becomes a system with its own constraints:

- data flow
- state management
- integration points

Thinking only in terms of pages or components is not enough.

---

## 8. Developer experience is part of the architecture

If the project is hard to work on:

- onboarding slows down
- mistakes increase
- productivity drops

Tooling, structure and clarity have a direct impact on this.

---

## 9. Prefer incremental changes over rewrites

Large rewrites are expensive and risky.

It is usually better to evolve the system step by step, even if it takes longer.

---

## 10. Accept trade-offs explicitly

Every decision has a cost.

It is better to be aware of it than to hide it behind abstractions or tools.

---

## Notes

These principles come from experience on systems with:

- multiple teams
- shared codebases
- long lifecycle

They are meant to support decisions, not replace them.