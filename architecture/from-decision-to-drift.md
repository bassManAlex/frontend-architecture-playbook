---
last-updated: 2026-07-01
target-stack: Framework-agnostic (illustrated with Next.js/React field notes)
status: Accepted
---

# From Decision to Drift

## Why this document exists

Every ADR in this repository describes an intention at time zero: the
decision, the rationale, the alternatives discarded. None of them describe
what happens afterward, as a project grows, priorities shift, and teams
rotate. Observing a real-world project that followed these same principles,
a recurring pattern emerges: **the original decision is rarely reversed
formally. It simply stops being applied to most new work, while remaining
technically true on paper.** This document tries to make that drift visible,
stage by stage, and offer a criterion for deciding whether to correct course
or update the decision itself.

All examples below are anonymized field notes from a real project; no
project or client names are used.

---

## Stage 1: Early stage (few features, one team, clean decisions)

At this stage, the decisions recorded in the ADRs are applied almost
literally, because a single team writes all the code and the surface area is
small enough to keep the rationale in mind.

Signals observed at this stage in the real project:

- The SSR-first pattern from [ADR-001](../adr/adr-001-ssr-vs-spa.md) holds
  consistently: few pages, few exceptions.
- The first shared component (a server-paginated table) doesn't exist yet as
  an abstraction: each feature has its own local implementation, consistent
  with "don't abstract too early" ([frontend-principle.md](frontend-principle.md)).
- A single locale is active (`it`), a second one (`en`) is scaffolded but
  not yet a problem, because nobody is depending on it yet.

**What to look for to confirm you're still in this stage:** the number of
features/teams is low enough that a violation of a principle (e.g. the same
component duplicated three times) is visible by inspection, without needing
tooling to catch it.

---

## Stage 2: Growth (more features, more teams, shortcuts start paying off)

This is where it gets interesting: shortcuts **work**, in the short term,
and precisely because of that they accumulate without being flagged as a
problem.

Concrete evidence from this stage:

- The Redux-boilerplate-per-fetch pattern (see the field note in
  [ADR-003](../adr/adr-003-state-management.md#field-note-where-this-decision-drifted-in-practice))
  is born here: a team needs to fetch reference data, and copies the nearest
  existing slice instead of introducing the server-state layer described in
  [ADR-004](../adr/adr-004-data-fetching.md), because "that's already how it's
  done elsewhere in the project." Local consistency with existing code beats
  consistency with the ADR.
- The shared table component is finally extracted, but only after being
  duplicated roughly 20 times (see the field note in
  [frontend-principle.md](frontend-principle.md), principle 5): the "don't
  abstract too early" principle held, at the opposite cost (late abstraction,
  almost never).
- The second locale (`en`) keeps being maintained in parallel out of inertia.
  Nobody removes it, nobody activates it (see
  [internationalization.md](internationalization.md)).

**The warning sign at this stage is not the first violation of a principle:
it's the second one that looks like the first.** One isolated deviation is
normal; one Redux-slice-per-fetch is an incident. Five identical slices are a
pattern nobody decided on but that has quietly become the de facto standard.
This is the point where an external review (done against documentation, not
code) and the reality of the codebase start to diverge silently: the ADRs
stay "Accepted" and unchanged, but describe less and less of what happens in
new code.

---

## Stage 3: Maturity (the debt is structural, reversing it is expensive)

By this point, the deviations observed in Stage 2 are no longer manageable
exceptions. They are the only way new hires learn the codebase, because
that's what they see everywhere in the existing code.

Concrete evidence:

- Roughly 85% of components are client-side (see the field note in
  [ADR-001](../adr/adr-001-ssr-vs-spa.md#field-note-how-ssr-as-default-looked-in-practice)):
  this is no longer "SSR by default with exceptions," it's close to the
  opposite. A new developer reading ADR-001 and then opening the codebase
  finds a reality close to inverted from what's described.
- The real coverage gate (70%, see the field note in
  [nextjs-architecture.md](nextjs-architecture.md)) is lower than the figure
  originally stated in the architecture document (95%): someone, at some
  point, had to lower it to make it reachable, but the document never told
  that story.
- Security matured unevenly: CSRF is implemented carefully (double-submit
  cookie, transparent refresh, see the field note in
  [security-beyond-csrf.md](security-beyond-csrf.md)), while CSP does not
  exist. This is likely because CSRF had a visible, immediate risk (form
  submission), while CSP requires an investment nobody ever prioritized
  without an incident to justify it.

**The criterion for deciding whether to correct course or accept the drift
as the new standard, at this stage:**

- If the drift has a measurable, recurring cost (e.g. the Redux boilerplate
  repeated every time new server state is needed) → it's worth correcting,
  because the cost grows linearly with every new feature.
- If the drift is dormant infrastructure with low maintenance cost (e.g. the
  `en` locale never activated) → it's usually better to decide explicitly to
  remove or activate it, not leave it in limbo: the cost here isn't
  technical, it's the confusion of whoever arrives later and can't tell if
  it's "almost ready" or "dead."
- If the drift reflects a real, undeclared priority (e.g. CSRF yes, CSP no)
  → the document should be updated to reflect the real priority, instead of
  leaving the false impression that "security" is a single, already-covered
  block.

---

## How to use this document

This is not a fourth ADR and it does not override the decisions recorded in
ADR-001 through ADR-004. It is a lens for re-reading them: when reviewing
this repository against a real, running project, check which stage each
decision is actually in — not which stage its ADR assumes it's in — before
deciding whether to enforce, update, or formally revise it.
