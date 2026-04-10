# Frontend Architecture Playbook

This repository contains documentation of frontend architectures used in real-world enterprise projects.

It is not a tutorial and it does not include production code.  
The goal is to document how systems were designed, what decisions were made, and what trade-offs were accepted.

---

## Context

The material here comes from projects involving:

- multi-tenant platforms (20+ organizations)
- public administration systems
- applications handling significant traffic (100K+ daily transactions)

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
- what worked and what didn’t

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