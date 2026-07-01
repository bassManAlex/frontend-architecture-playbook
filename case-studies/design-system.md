---
last-updated: 2026-07-01
target-stack: React 18 (framework-agnostic component library)
status: Accepted
---

# Design System for Distributed Teams

## Context

The design system was introduced in a context where multiple teams were working on related applications, but without a shared UI foundation.

The environment included around 15 research institutes, each with its own needs and timelines, but with overlapping requirements in terms of interface and behavior.

---

## Initial Situation

Before the design system:

- components were duplicated across projects
- similar features were implemented in slightly different ways
- UI inconsistencies were common
- onboarding new developers required understanding multiple codebases

There was no clear ownership of shared components.

---

## Objective

The goal was not to build a “component library” in isolation, but to:

- reduce duplication
- improve consistency across applications
- make teams more independent
- avoid re-implementing the same patterns

---

## Approach

### Start from real use cases

Instead of designing components upfront, the system was built incrementally:

- components were extracted from real features
- only reused elements were promoted to shared components

This avoided over-engineering early on.

---

### Define a shared base

A set of reusable components was introduced, covering:

- form inputs
- layout elements
- basic interaction patterns

Over time, this grew to 50+ components.

---

### Keep feature ownership local

Not everything was moved into the design system.

Rule of thumb:

- if something is used in multiple places → shared
- otherwise → stays inside the feature

This prevented the shared layer from becoming too generic.

---

### Versioning & distribution

Field note from a real-world project following this same approach: the design
system was published as an ordinary versioned npm package to a private registry
(GitHub Packages), consumed via a pinned semver range:

```
@scope/design-system-ui-kit: ^5.22.3
```

```
@scope:registry=https://npm.pkg.github.com
```

It was not distributed as an in-repo workspace package. Consuming
applications pulled it in the same way they would any third-party dependency,
including going through a version bump (and a changelog check) to pick up
changes. This is a meaningfully different trade-off from an in-repo shared
package: consumers get isolation from unreviewed breaking changes, at the cost
of every fix requiring a publish-and-bump cycle instead of being immediately
visible. See also the note on this in
[ADR-002](../adr/adr-002-modorepo-choice.md#field-note-what-monorepo-meant-in-practice-on-a-real-project).

---

### Documentation (minimal but necessary)

Documentation was added mainly to:

- explain how components should be used
- avoid incorrect usage

It was kept simple and updated only when needed.

---

### Integration with existing systems

The design system had to work with existing platforms (e.g. CMS-based setups).

This introduced some constraints:

- components had to be adaptable
- not all patterns could be fully controlled

---

## Trade-offs

- maintaining shared components requires coordination
- some teams preferred local implementations for speed
- documentation needs continuous updates to stay useful

---

## What worked

- duplication across projects went down, though we don't have precise before/after numbers
- consistency in UI and behavior improved, based on fewer one-off implementations reported by teams
- onboarding for new developers got easier, in the experience of the teams involved
- teams could reuse components instead of rebuilding them

---

## What didn’t work well

- initial lack of ownership created confusion
- some components became too generic over time
- alignment across teams required continuous effort

---

## Notes

The design system is not a fixed product.

It evolves with the applications and the teams using it.

Trying to fully standardize everything early would not have worked in this context.