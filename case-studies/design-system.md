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

- reduced duplication across projects
- improved consistency in UI and behavior
- easier onboarding for new developers
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