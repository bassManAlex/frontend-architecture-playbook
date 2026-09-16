---
last-updated: 2026-09-16
target-stack: mixed, see per-document target-stack
status: Accepted
---

# Frontend Architecture Playbook

This repository contains documentation of frontend architectures used in real-world enterprise projects.

It is not a tutorial and it does not include production code.
The goal is to document how systems were designed, what decisions were made, and what trade-offs were accepted.

Each document is a dated snapshot of a specific engagement, not a description of current practice. Stack versions differ between documents and most were written against Next.js 13/14 (App Router) and React 18; current work is on Next.js 16 and React 19. Every document states its own target stack in the header, and each carries a stack note near the top.

---

## Context

The material here comes from more than one engagement, and figures below are not all from the same project:

- one case study covers a multi-tenant platform serving 20+ public organizations
- other documents describe a narrower, single-organization public administration system
- some figures (such as 100K+ daily transactions) apply to the larger multi-tenant engagement specifically, not to every project referenced in this repository

All proprietary details have been removed, but the structure and decisions reflect real implementations.

---

## Repository Structure

### case-studies/

Concrete examples of systems that were built:

- Next.js multi-tenant platform
- shared design system across multiple teams

Each document focuses on:

- initial problem
- decisions taken
- what worked and what didn't

---

### architecture/

More structured documentation of how applications are organized.

This includes:

- Next.js architecture (App Router, API layer, modules)
- general frontend principles used across projects

The goal is not to describe every detail, but to explain how the system is structured and why.

---

### adr/

Architectural Decision Records.

These are short documents describing specific technical decisions, for example:

- SSR vs SPA
- monorepo vs multiple repositories
- state management approach

Each ADR explains the context, the decision, and the consequences.

---

## Index

10 documents below are complete and accepted. The 4 documents in "In progress" further down are skeletons awaiting verified project input and are intentionally excluded from this count.

| Document | Folder | Status | Last updated | Target stack |
|---|---|---|---|---|
| [ADR-001: SSR vs SPA](adr/adr-001-ssr-vs-spa.md) | adr/ | Accepted | 2026-07-01 | Next.js 13/14 (App Router), React 18 |
| [ADR-002: Monorepo](adr/adr-002-modorepo-choice.md) | adr/ | Accepted | 2026-07-01 | Next.js 13/14 (App Router), pnpm workspaces |
| [ADR-003: State Management](adr/adr-003-state-management.md) | adr/ | Accepted | 2026-07-01 | Next.js 13/14 (App Router), React 18, React Query, Redux |
| [ADR-004: Data Fetching Strategy](adr/adr-004-data-fetching.md) | adr/ | Accepted | 2026-07-01 | Next.js 13/14 (App Router), React 18, React Query |
| [Frontend Principles](architecture/frontend-principle.md) | architecture/ | Accepted | 2026-07-01 | Framework-agnostic |
| [Next.js Frontend Architecture](architecture/nextjs-architecture.md) | architecture/ | Accepted | 2026-07-01 | Next.js 13/14 (App Router), React 18 |
| [Scaling a Multi-Tenant Frontend Platform with Next.js](architecture/nextjs-multi-tenant-platform.md) | architecture/ | Accepted | 2026-07-01 | Next.js 13/14 (App Router), React 18 |
| [From Decision to Drift](architecture/from-decision-to-drift.md) | architecture/ | Accepted | 2026-07-01 | Framework-agnostic |
| [Design System for Distributed Teams](case-studies/design-system.md) | case-studies/ | Accepted | 2026-07-01 | React 18 (framework-agnostic component library) |
| [Next.js Multi-Tenant Platform](case-studies/next-platform.md) | case-studies/ | Accepted | 2026-07-01 | Next.js 13/14 (App Router), React 18 |

### In progress (not counted above)

These are skeletons with open questions, not finished documents. They stay out of the repository's public count until the open questions are answered from a real project and the content can be verified, per the note in each file.

| Document | Folder | Status | Last updated | Target stack |
|---|---|---|---|---|
| [Accessibility & Public Administration Compliance](architecture/accessibility-pa-compliance.md) | architecture/ | Draft | 2026-07-01 | TBD: pending input |
| [Internationalization](architecture/internationalization.md) | architecture/ | Draft | 2026-07-01 | TBD: pending input |
| [Performance & Core Web Vitals](architecture/performance-core-web-vitals.md) | architecture/ | Draft | 2026-07-01 | TBD: pending input |
| [Security Beyond CSRF](architecture/security-beyond-csrf.md) | architecture/ | Draft | 2026-07-01 | TBD: pending input |

---

## Why this exists

In large frontend systems, most problems are not about components or frameworks.

They are about:

- keeping multiple teams aligned
- avoiding duplication
- maintaining consistency over time

This repository tries to capture those aspects.

---

## Notes

- no proprietary code is included
- examples are simplified but not fictional
- focus is on architecture, not implementation details
- documents reflect the stack and scope in place at the time each engagement happened, not current recommendations
