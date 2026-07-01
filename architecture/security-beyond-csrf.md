---
last-updated: 2026-07-01
target-stack: TBD: pending input
status: Draft
---

# Security Beyond CSRF

This document is a skeleton. It has no verified technical content yet; see
"Open questions" below for what is needed before this can be written.

CSRF and OIDC-based authentication are already mentioned in
[nextjs-architecture.md](nextjs-architecture.md) and
[nextjs-multi-tenant-platform.md](nextjs-multi-tenant-platform.md). This document
is meant to cover what those two do not: CSP, XSS sanitization, secrets handling,
and dependency scanning.

---

## Context

*(to fill in)*

---

## Content Security Policy

*(to fill in: is a CSP actually configured? Strict or report-only? Any known
constraints from the multi-tenant/CMS integration mentioned in
[design-system.md](../case-studies/design-system.md))*

### Field note: CSRF pattern observed in a real project (not CSP)

A real-world project in this same space implements CSRF protection as a
double-submit cookie pattern enforced at the proxy/middleware layer, ahead of
the BFF route handlers:

```ts
const CSRF_COOKIE_NAME = "csrf_token";
const CSRF_HEADER_NAME = "x-csrf-token";
const STATE_CHANGING_METHODS = ["POST", "PUT", "DELETE", "PATCH"];
const CSRF_PROTECTED_PATHS = ["/api/"];
```

State-changing requests to protected paths must present a header matching the
cookie value; a documented exemption list covers public/webhook routes that
should not require it. This is a concrete, verifiable implementation of the
CSRF protection this repository's other documents mention only in passing.

No CSP configuration was found in this same project's `next.config.ts` (no
`headers()` entry, no CSP-related middleware logic). This is not evidence
that no project in this space has a CSP — only that, in the one instance
checked, CSP and CSRF protection are not the same maturity level: CSRF is
implemented and testable, CSP was not found configured at the framework
level. Question 1 below remains open for any other project.

---

## XSS & Input Sanitization

*(to fill in: what sanitization is applied to user-generated or tenant-provided
content, if any)*

---

## Secrets Handling

*(to fill in: how client vs server secrets are managed, especially relevant given
the BFF/API proxy layer described in
[nextjs-multi-tenant-platform.md](nextjs-multi-tenant-platform.md))*

---

## Dependency Scanning

*(to fill in: is there an automated tool, e.g. Dependabot, Snyk, npm audit in CI, and
is it a hard gate or advisory?)*

---

## Trade-offs

*(to fill in)*

---

## Open questions

These need real answers from the project before this document can contain
verified content instead of generic claims:

1. Is a Content Security Policy actually enforced (via headers/middleware), and is
   it strict or report-only? Any exceptions required by tenant-specific
   embeds/CMS integration?
2. What sanitization library or approach is used for any tenant- or user-provided
   content rendered in the UI?
3. How are secrets (API keys, OIDC client secrets) managed for the BFF/API proxy
   layer described in
   [nextjs-multi-tenant-platform.md](nextjs-multi-tenant-platform.md)? Environment
   variables per tenant, a secrets manager, something else?
4. Is dependency scanning part of the CI pipeline referenced in
   [nextjs-architecture.md](nextjs-architecture.md) (alongside test coverage and
   linting), or is it not currently automated?
5. Beyond CSRF, is there a documented security incident, audit finding, or
   near-miss that would make this document concrete rather than a generic
   security checklist?
