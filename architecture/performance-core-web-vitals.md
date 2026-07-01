---
last-updated: 2026-07-01
target-stack: TBD: pending input
status: Draft
---

# Performance & Core Web Vitals

This document is a skeleton. It has no verified technical content yet; see
"Open questions" below for what is needed before this can be written.

---

## Context

*(to fill in: what performance constraints or targets exist for the multi-tenant
platform described in [nextjs-multi-tenant-platform.md](nextjs-multi-tenant-platform.md)
and [next-platform.md](../case-studies/next-platform.md))*

---

## Goals & Constraints

*(to fill in)*

---

## Approach

*(to fill in: caching strategy, e.g. fetch cache, ISR, revalidation; bundle splitting;
image optimization; per-tenant considerations)*

---

## Metrics & Monitoring

*(to fill in: what is actually measured in production, if anything, and how)*

---

## Trade-offs

*(to fill in)*

---

## What worked / What didn't work well

*(to fill in, following the same honest format used in the existing case studies)*

---

## Open questions

These need real numbers or confirmed decisions from the project before this
document can contain verified content instead of generic claims:

1. Are Core Web Vitals (LCP, INP, CLS) actually measured in production for these
   applications? If so, with what tool (e.g. Vercel Analytics, a custom RUM setup,
   Lighthouse CI in the pipeline)?
2. What caching strategy is used for data on the App Router (fetch cache, ISR,
   on-demand revalidation)? [ADR-004](../adr/adr-004-data-fetching.md) covers the
   client-side/React Query layer but not server-side caching semantics.
3. Is there a bundle size budget or bundle analysis step in CI? Any known numbers
   (e.g. current bundle size per tenant, or a specific regression that was caught
   or missed)?
4. Given the platform is multi-tenant, does per-tenant configuration or theming
   affect bundle size or hydration cost in a measurable way?
5. Is there a documented performance incident or regression (a near-failure, similar
   in spirit to the "what didn't work" sections elsewhere) that would be more
   informative here than a generic best-practices list?
6. What runtime is used for deployment, Node.js or Edge, and was this a deliberate
   performance decision or a default?
