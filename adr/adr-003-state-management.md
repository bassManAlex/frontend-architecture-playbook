---
last-updated: 2026-07-01
target-stack: Next.js 13/14 (App Router), React 18, React Query, Redux
status: Accepted
---

# ADR-003: State Management

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

---

## Field note: where this decision drifted in practice

A real-world project following this same architecture ended up with server data
(reference/lookup data fetched from an API) living inside Redux slices, not outside
global stores as this ADR prescribes. A representative shape (anonymized, field
names changed):

```ts
export const fetchReferenceData = createAsyncThunk(
  "referenceData/fetch",
  async () => {
    const response = await referenceDataApi.list();
    return response.items;
  },
  {
    condition: (_, { getState }) => {
      const status = (getState() as RootState).referenceDataSlice?.status ?? "idle";
      return status === "idle" || status === "failed";
    },
  },
);

const referenceDataSlice = createSlice({
  name: "referenceData",
  initialState: { data: [], status: "idle" as const, error: undefined as string | undefined },
  reducers: {
    invalidate: (state) => { state.status = "idle"; },
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchReferenceData.pending, (state) => { state.status = "loading"; })
      .addCase(fetchReferenceData.fulfilled, (state, action) => {
        state.status = "succeeded";
        state.data = action.payload;
      })
      .addCase(fetchReferenceData.rejected, (state, action) => {
        state.status = "failed";
        state.error = action.error.message;
      });
  },
});
```

This pattern was repeated, nearly identically, across several unrelated slices
(each one re-implementing `idle/loading/succeeded/failed`, an error string, and a
manual `condition` guard to avoid refetching). This is the cost this ADR warned
about under "Trade-offs" ("multiple patterns in the same project... developers
need to understand when to use each one") showing up as the opposite problem:
not multiple patterns, but one pattern used outside its intended boundary, copied
by hand instead of centralized.

Applying this ADR as written would mean this data belongs in the server-state
layer described in [ADR-004](adr-004-data-fetching.md) (React Query), which
already provides `idle/loading/success/error` status and refetch-guarding for
free, removing the need to hand-write it per slice.