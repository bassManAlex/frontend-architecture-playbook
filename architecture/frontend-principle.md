---
last-updated: 2026-07-01
target-stack: Framework-agnostic
status: Accepted
---

# Frontend Principles

This document collects a set of practical guidelines used when working on frontend systems at scale.

They are not strict rules, but recurring patterns that proved useful across different projects.

---

## 1. Prefer structure over speed (in the long run)

Quick solutions tend to accumulate and become harder to manage.

When the same pattern appears more than once, it usually means it should be structured properly.

Example: in [design-system.md](../case-studies/design-system.md), components were extracted from real features only once reused elsewhere, rather than designed upfront speculatively. This avoided over-engineering while still moving toward proper structure.

---

## 2. Consistency matters more than local optimization

Different teams solving the same problem in different ways creates friction.

Even if a solution is not perfect, consistency across the codebase usually pays off.

Example: in [next-platform.md](../case-studies/next-platform.md), "What didn't work well" notes that "some teams tried to bypass shared components for speed." The cost of prioritizing local optimization over consistency showed up directly as a maintenance problem.

---

## 3. Avoid mixing concerns

Try to keep clear boundaries between:

- domain logic
- shared utilities
- UI components

Once these start to overlap, changes become harder and side effects increase.

No direct example found in the existing case studies.

---

## 4. Keep features self-contained

Each feature should be understandable on its own.

A developer should be able to work on a feature without needing to navigate the entire project.

Example: in [next-platform.md](../case-studies/next-platform.md), the codebase is organized by feature, with each feature bundling its own components, hooks, API calls and types, specifically so a domain could be worked on without navigating the entire codebase.

---

## 5. Don’t abstract too early

Abstractions are useful, but only when there is something real to abstract.

If something is used only once, it probably doesn’t belong in a shared layer yet.

Example: in [design-system.md](../case-studies/design-system.md), the rule of thumb was explicit: "if something is used in multiple places → shared, otherwise → stays inside the feature," which kept the shared layer from becoming too generic.

Field note from a real-world project: a server-side paginated table component
(wrapping a design-system `DataTable`) was only promoted to the shared layer after
the same page/pageSize/total/onPageChange/isLoading contract had already appeared,
independently, across several features. By the time it was extracted, it was
already in active use in about 20 different places in the codebase, so the shared
component's API was dictated by real call sites, not designed upfront:

```tsx
export interface ServerDataTableProps<TData> {
  columns: DataTableColumnDef<TData, unknown>[];
  data: TData[];
  page: number;
  pageSize: number;
  total: number;
  onPageChange: (page: number) => void;
  onPageSizeChange: (size: number) => void;
  isLoading?: boolean;
  // ...pass-through props for empty state, sorting, styling variants
}
```

This is what "abstract only once something real exists to abstract" looks like
in practice: the shared component's shape is a direct trace of a pattern that
already existed multiple times, not a speculative generalization.

---

## 6. Shared code needs ownership

Reusable components and utilities don’t maintain themselves.

Without clear ownership, they tend to degrade over time.

Example: in [design-system.md](../case-studies/design-system.md), "What didn't work well" names "initial lack of ownership created confusion" as a direct consequence of skipping this.

---

## 7. Treat the frontend as a system, not just pages

As applications grow, the frontend becomes a system with its own constraints:

- data flow
- state management
- integration points

Thinking only in terms of pages or components is not enough.

No direct example found in the existing case studies.

---

## 8. Developer experience is part of the architecture

If the project is hard to work on:

- onboarding slows down
- mistakes increase
- productivity drops

Tooling, structure and clarity have a direct impact on this.

No direct example found in the existing case studies.

---

## 9. Prefer incremental changes over rewrites

Large rewrites are expensive and risky.

It is usually better to evolve the system step by step, even if it takes longer.

Example: in [next-platform.md](../case-studies/next-platform.md), the stated objective was explicit about this: "The goal was not just to 'rewrite' the frontend, but to reduce duplication... define a consistent structure... [and] make onboarding of new tenants faster" incrementally.

---

## 10. Accept trade-offs explicitly

Every decision has a cost.

It is better to be aware of it than to hide it behind abstractions or tools.

Example: both [design-system.md](../case-studies/design-system.md) and [next-platform.md](../case-studies/next-platform.md) dedicate an explicit "Trade-offs" section listing costs accepted alongside each decision, rather than presenting the decisions as free wins.

---

## Notes

These principles come from experience on systems with:

- multiple teams
- shared codebases
- long lifecycle

They are meant to support decisions, not replace them.