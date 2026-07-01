---
last-updated: 2026-07-01
target-stack: TBD: pending input
status: Draft
---

# Accessibility & Public Administration Compliance

This document is a skeleton. It has no verified technical content yet; see
"Open questions" below for what is needed before this can be written.

---

## Context

*(to fill in: which regulatory framework applies, e.g. AgID guidelines, WCAG version
and conformance level, and whether compliance is a contractual/legal obligation
for the projects described in this repository)*

---

## Goals & Constraints

*(to fill in)*

---

## Approach

*(to fill in: how accessibility is addressed in practice, e.g. design system level,
application level, or both)*

---

## Testing & Verification

*(to fill in: manual audit process, if any, and whether a specific WCAG
conformance level is formally verified — see field note below for what
automated tooling looks like in practice)*

### Field note: automated a11y testing observed in a real project

A real-world project in this same space had automated accessibility testing
wired into both unit and end-to-end tests, using `jest-axe` for
component-level checks and `@axe-core/playwright` for end-to-end runs, with
roughly 34 test files asserting `toHaveNoViolations()` or equivalent. This
confirms automated a11y testing is a realistic, adoptable practice in this
kind of codebase. It does not by itself confirm a specific WCAG conformance
level, only that violations caught by axe's ruleset are checked in CI-run
tests rather than left to manual review alone. Question 3 below still needs
an answer on whether this is gate-enforced (build fails on violation) or
advisory (violations logged but not blocking).

---

## Trade-offs

*(to fill in)*

---

## What worked / What didn't work well

*(to fill in, following the same honest format used in the existing case studies)*

---

## Open questions

These need real answers from the project before this document can contain
verified content instead of generic claims:

1. Which conformance target applies, e.g. WCAG 2.1 AA, WCAG 2.2 AA, or an AgID-specific
   profile? Is this a contractual requirement or a best-effort goal?
2. Was accessibility built into the design system's shared components (case study:
   [design-system.md](../case-studies/design-system.md)) from the start, or retrofitted
   later? If retrofitted, what prompted it?
3. Is there automated testing in CI (e.g. axe-core, Lighthouse CI) enforcing any
   threshold, or is verification manual/ad hoc?
4. Are there known gaps or components that do not meet the target conformance level
   today? A documented "what didn't work" here would be more valuable than a
   claim of full compliance.
5. Who owns accessibility decisions, e.g. a central team, each feature team, or is it
   unowned in practice (echoing the "shared code needs ownership" principle in
   [frontend-principle.md](frontend-principle.md))?
