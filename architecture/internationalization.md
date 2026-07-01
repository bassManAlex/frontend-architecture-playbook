---
last-updated: 2026-07-01
target-stack: TBD: pending input
status: Draft
---

# Internationalization

This document is a skeleton. It has no verified technical content yet; see
"Open questions" below for what is needed before this can be written.

This is a gap the original review flagged directly: internationalization is
relevant for a multi-tenant public-administration context but was never
mentioned anywhere in this repository.

---

## Context

*(to fill in: is i18n a requirement across all tenants, or specific to some?
Is it driven by a regulatory need, e.g. minority language support, or a
product decision?)*

---

## Approach

*(to fill in: message catalog structure, per-feature vs per-page organization,
how translation keys are namespaced)*

### Field note: an i18n setup that never fully activated

A real-world project in this same space used `next-intl` with a
feature-namespaced message catalog: two complete locale directories (`it`,
`en`), each with the same 68 JSON files, one per feature/domain
(`dashboard.json`, `berthBooking.json`, `companies.json`, and so on).
Structurally, it was a solid, maintained setup.

In practice, the active locale was hardcoded at build time:

```ts
/** App locale: statically set to Italian until next-intl becomes dynamic. */
export const APP_LOCALE = "it" as const;
```

Both catalogs were kept in sync (same 68 files on each side), but there was no
language switcher in the UI, and the `en` catalog was never served to a real
user. The infrastructure for i18n existed and was maintained; the feature it
was built for (letting a user actually choose English) was not shipped.

This is a useful case for what "scaling in the wrong direction" can look
like: effort went into keeping two catalogs current (a real, recurring cost)
for a switch that was never turned on. Whether that was the right call
depends on information this document doesn't have yet — see open questions.

---

## Pluralization, Formatting & RTL

*(to fill in: date/number/currency formatting per locale, plural rules, and
whether right-to-left languages are a real requirement or out of scope)*

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

1. Is multi-language support an active requirement (regulatory or contractual)
   for any tenant today, or was the second locale prepared for a future need
   that hasn't materialized yet?
2. If a second locale is maintained but not served, what triggers translating
   a new string into it — is it done as new features ship, or is there a
   backlog/lag? Concretely: is the `en` catalog actually current, or is "same
   68 files on each side" a snapshot that has already started to drift?
3. Was there ever a language switcher planned in the UI, and if so, what
   stopped it from shipping?
4. Is locale selection expected to be per-tenant (e.g. some public
   administrations require a second language, others don't), and if so, does
   the current single-locale-at-build-time approach support that at all?
5. Who owns translation content, e.g. engineering, a content/localization team, or
   is it unowned in practice (same ownership question raised for the design
   system in [design-system.md](../case-studies/design-system.md) and
   principle 6 in [frontend-principle.md](frontend-principle.md))?
