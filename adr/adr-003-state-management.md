# ADR-003 — State Management

## Status

Accepted

---

## Context

The application needs to handle different types of state:

- authentication (user, token, permissions)
- UI configuration (theme, language)
- server data coming from APIs

Using a single approach for everything was considered, but it quickly became hard to manage.

---

## Decision

Split state management based on responsibility:

- Context for authentication
- Redux for UI-related state
- server data handled outside global stores

---

## Alternatives

### Single global store (Redux for everything)

Pros:

- centralized state
- predictable flow

Cons:

- too much unrelated data in one place
- harder to manage volatile state (auth, tokens)

---

### Context only

Pros:

- simple
- no external dependencies

Cons:

- does not scale well for larger applications
- limited tooling

---

### Server-state library only

Pros:

- good for data fetching and caching

Cons:

- does not cover UI or authentication
- still requires additional layers

---

## Rationale

Different types of state behave differently.

Trying to force everything into a single solution adds complexity.

Keeping them separated makes it easier to reason about:

- what changes often (auth)
- what is stable (UI)
- what comes from the backend (server data)

---

## Trade-offs

- multiple patterns in the same project
- developers need to understand when to use each one

---

## Consequences

Positive:

- clearer separation of concerns
- less unnecessary global state
- more flexibility

Negative:

- requires some discipline
- not as straightforward as a single solution

---

## Notes

This is not meant to be a strict rule.

If the application grows, this approach may need adjustments.