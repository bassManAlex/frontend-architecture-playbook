---
last-updated: 2026-07-01
target-stack: Next.js 13/14 (App Router), React 18
status: Accepted
---

# Next.js Frontend Architecture

## Overview

This architecture is designed for scalable, multi-tenant frontend platforms using Next.js and React.

## Architecture Goals & Constraints

### Goals

- Support a multi-tenant platform serving multiple independent organizations
- Enable modular development across feature teams
- Ensure consistency across applications through shared standards
- Provide secure and scalable integration with backend microservices

### Constraints

- Integration with existing enterprise systems (OIDC, microservices)
- Strict security requirements (CSRF, session handling)
- Need for both flexibility (tenant-specific behavior) and standardization
- Teams with mixed seniority levels

### Non-Goals

- Building a fully generic framework
- Supporting rapid prototyping at the expense of structure

## Core Principles

- Modularity
- Reusability
- Scalability across multiple applications
- Strong separation of concerns

## Architecture Components

### 1. Application Layer

- Next.js applications (SSR + Route Handlers)
- Tenant-specific configuration
- Routing and page composition

### 2. Shared Libraries

- UI component library (design system)
- Utility functions
- Shared business logic

Managed via:
- pnpm workspaces

### 3. State Management

- Server state: React Query
- Client state: Context API / Redux (when needed)

### 4. API Layer

- Next.js Route Handlers
- Integration with backend microservices

### 5. CI/CD

- Automated pipelines
- Quality gates:
  - Test coverage (>80%), used as a signal, not a hard target. A much higher threshold
    (e.g. >95%) tends to incentivize tautological tests written to inflate the number
    rather than to catch real regressions.
    - Field note: a real-world project following this same rationale settled on a
      70% threshold across statements/branches/functions/lines, with layout/page
      shells and test files explicitly excluded from the measurement. That is a
      concrete data point below the >80% figure above, supporting the same
      "signal, not target" reasoning.
  - Linting
  - Build validation

## Key Decisions

- SSR for control over data loading, not for SEO (pages are behind OIDC authentication and not indexable)
- Monorepo for code sharing and consistency
- Design system as a core architectural pillar

## Trade-offs

- Increased complexity in build and deployment
- Higher initial setup cost
- Requires strong governance

## When to Use This Architecture

- Multi-tenant platforms
- Large teams
- High consistency requirements
- Long-term maintainability focus

## When NOT to Use

- Small projects
- Single-tenant apps
- Rapid prototypes
