---
last-updated: 2026-07-01
target-stack: Next.js 13/14 (App Router), pnpm workspaces
status: Accepted
---

# ADR-002: Monorepo

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

---

## Field note: what "monorepo" meant in practice on a real project

A real-world project following this ADR's rationale ("single place for shared
components and common logic") did not end up with a multi-package monorepo. Its
`pnpm-workspace.yaml` declared a single package:

```yaml
packages:
  - .
```

The shared UI component library was not a workspace package at all: it was
consumed as an ordinary versioned dependency, published to a private npm
registry and pinned with a semver range in `package.json`:

```
@scope/design-system-ui-kit: ^5.22.3
```

```
@scope:registry=https://npm.pkg.github.com
```

In other words, "monorepo" here described the application's internal
organization (features, shared utilities within the same app), not a
multi-package build graph. The shared design system followed the polyrepo model
this ADR lists as an alternative (separate versioning, separate release cadence,
consumed as a dependency) rather than being a workspace package inside this
repository.

This does not contradict the ADR's rationale (reducing duplication, keeping a
single place for shared logic within the application) but it does mean the
"Decision" section's phrase "monorepo structure with shared packages" should not
be read as "every shared piece of code lives in this repository as a workspace
package." A design system consumed as a versioned external dependency has its
own trade-offs (see the design system distribution details in
[design-system.md](../case-studies/design-system.md)) that are different from
the ones this ADR discusses for in-repo shared packages.