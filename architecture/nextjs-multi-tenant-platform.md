---
last-updated: 2026-07-01
target-stack: Next.js 13/14 (App Router), React 18
status: Accepted
---

# Scaling a Multi-Tenant Frontend Platform with Next.js

> **Stack note.** This document is a historical record of a completed engagement built on Next.js 13/14 (App Router), React 18. It is not a description of current practice; current work uses Next.js 16 and React 19.
>
> **Scope note.** This document describes a different, larger-scope engagement than other public-sector work referenced elsewhere in this repository. The two should not be read as the same client or the same deployment scope.

## Context

A platform serving 20+ independent government authorities, each requiring:
- Custom configurations
- Shared UI standards
- Independent deployments

## Problem

- High duplication across projects
- Slow onboarding for new tenants (weeks)
- Inconsistent UI/UX
- Lack of shared engineering standards

## Architecture Decisions

- Adopted Next.js with SSR for flexibility and performance
- Introduced a monorepo structure using pnpm workspaces
- Implemented shared component libraries as pnpm workspace packages
- Standardized Route Handler patterns
- Established CI/CD pipelines with strict quality gates

## Trade-offs

- SSR increased complexity in deployment and caching
- Monorepo required stricter dependency management
- Shared libraries introduced versioning challenges

## Impact

- Platform scaled to 20+ tenants
- Onboarding time went from weeks to days in practice, though we don't have precise per-tenant numbers
- Consistency across applications improved, based on fewer one-off UI implementations reported by teams
- Distributed teams were able to work independently on their own tenant configurations

## Key Takeaways

- Multi-tenant frontend platforms seemed to benefit from strong standardization, based on this project's experience
- A shared design system helped scaling teams here, though we didn't measure this against an alternative
- Architecture needs to balance flexibility with maintainability
