---
last-updated: 2026-07-01
target-stack: Next.js 13/14 (App Router), React 18
status: Accepted
---

# Next.js Multi-Tenant Platform

> **Stack note.** This document is a historical record of a completed engagement built on Next.js 13/14 (App Router), React 18. It is not a description of current practice; current work uses Next.js 16 and React 19.
>
> **Scope note.** This case study documents a different, larger-scope engagement than other public-sector work referenced elsewhere in this repository. The two should not be read as the same client or the same deployment scope.

## Context

The platform was built to support multiple public organizations (20+), each with its own operational context but sharing a common frontend foundation.

From a user perspective, each tenant behaves like an independent application.  
From a technical perspective, the goal was to avoid duplicating the same system multiple times.

The system handles a significant amount of daily traffic and is used by different roles (administrative users, operators, internal staff).

---

## Initial Situation

Before introducing a shared platform:

- multiple frontend projects existed with overlapping logic
- UI and UX were inconsistent across applications
- onboarding a new tenant required setting up a new project from scratch
- no shared standards for testing, structure or deployment

This made maintenance expensive and slowed down delivery.

---

## Objective

The goal was not just to “rewrite” the frontend, but to:

- reduce duplication across tenants
- define a consistent structure for all applications
- allow teams to work on different parts without stepping on each other
- make onboarding of new tenants faster

---

## Key Decisions

### Next.js as the base

Next.js was chosen mainly to consolidate responsibilities in one place:

- routing
- rendering
- API layer

SSR was not introduced for SEO reasons, but to have more control over how data is loaded and handled.

---

### Multi-tenant structure

Instead of creating separate projects per tenant, the platform was structured so that:

- core logic is shared
- tenant-specific behavior is handled via configuration

This reduced duplication but introduced the need for clear boundaries between shared and custom code.

---

### Monorepo

A monorepo approach was adopted to keep everything aligned:

- shared components
- utilities
- application code

This simplified reuse, but required discipline to avoid coupling everything together.

---

### Feature-based structure

The codebase is organized by features, not by technical layers.

Each feature contains:

- components
- hooks
- API calls
- types

This made it easier to work on a domain without navigating the entire codebase.

---

### API proxy (BFF)

A server-side layer was introduced using Next.js to:

- centralize API calls
- handle authentication
- apply basic security (CSRF)

This avoids exposing backend details directly to the client.

---

## Trade-offs

Some of the decisions above introduced complexity:

- SSR makes debugging less straightforward compared to a pure SPA
- monorepo requires coordination between teams
- shared code can become a bottleneck if ownership is not clear

These were accepted because the alternative (multiple independent apps) would not scale.

---

## What worked

- onboarding time for new tenants went down, though we don't have precise numbers
- shared components seemed to improve consistency across applications, based on fewer one-off implementations reported by teams
- teams could reuse existing logic instead of rewriting it

---

## What didn’t work well

- initial monorepo setup lacked clear ownership rules
- some teams tried to bypass shared components for speed
- debugging SSR-related issues required additional effort

---

## Notes

This is not a “generic” architecture.

Some decisions are specific to:

- public administration constraints
- integration with existing systems
- long-term maintenance requirements

The main focus was stability and consistency, not flexibility at all costs.