---
title: React – Fundamentals, Internals & Patterns
tags: [react, javascript, hooks, virtual-dom, state-management, fiber]
sources: [sources/reactjs-study-plan.md]
last-updated: 2026-04-17
---

# React – Fundamentals, Internals & Patterns

## Core Mental Model

React is **declarative** — you describe what the UI should look like for a given state. React handles *how* to update the DOM efficiently.

```jsx
// Declarative: describe the desired state
function Counter({ count }) {
  return <div>{count > 0 ? <span>{count}</span> : <span>Empty</span>}</div>;
}
```

## Virtual DOM & Reconciliation

**Why Virtual DOM?**
Direct DOM manipulation triggers browser reflow + repaint (expensive). VDOM is a lightweight JS object — cheap to create and diff.

**Reconciliation algorithm (O(n), not O(n³)):**

Two key assumptions:
1. **Elements of different types** → destroy entire subtree, build new one from scratch
2. **`key` prop** → identifies stable elements across renders; prevents incorrect reuse in lists

```jsx
// Without keys: React compares by index → broken on reorder
// With keys: React tracks by identity → correct
{items.map(item => <Item key={item.id} data={item} />)}
```

## React Fiber

- Internal reimplementation of reconciler (React 16+)
- Enables **incremental rendering** — work can be paused, resumed, prioritized
- **Concurrent Mode** (`<React.StrictMode>`, `startTransition`) allows low-priority renders to be interrupted
- `startTransition(() => setQuery(input))` marks state update as non-urgent

## Hooks

| Hook | Purpose | Key Gotcha |
|------|---------|-----------|
| `useState` | Local state | State updates are asynchronous; batched in React 18 |
| `useEffect` | Side effects | Cleanup runs on unmount AND before re-running; empty `[]` = run once |
| `useRef` | Mutable value without re-render; DOM ref | Changing `.current` doesn't trigger re-render |
| `useMemo` | Memoize expensive computation | Only when computation is genuinely costly |
| `useCallback` | Stable function reference | Needed when passing callbacks to `React.memo` children |
| `useReducer` | Complex state logic | Alternative to multiple `useState` calls |
| `useContext` | Consume context | Re-renders ALL consumers when context value changes |

**`useEffect` cleanup:**
```jsx
useEffect(() => {
  const sub = subscribe(userId);
  return () => sub.unsubscribe();  // runs on unmount and before next effect
}, [userId]);
```

## Performance Optimization

| Technique | What It Does |
|-----------|-------------|
| `React.memo(Component)` | Skip re-render if props shallowly equal |
| `useMemo` | Memoize computed value |
| `useCallback` | Stable function ref for memo'd children |
| `React.lazy + Suspense` | Code split; load component on demand |
| `react-window` / `react-virtual` | Virtualize long lists (only render visible rows) |

> [!warning] React.memo pitfall
> `React.memo` uses shallow equality. Object/array props always fail shallow equality — wrap creation in `useMemo` or move outside component.

## State Management

| Tool | Best For |
|------|---------|
| `useState` / `useReducer` | Local component state |
| Context API + `useReducer` | Small-medium apps; avoid for frequent updates |
| Redux Toolkit | Large apps; time-travel debugging; middleware |
| Zustand | Lightweight; no boilerplate; outside-React access |
| React Query / TanStack Query | **Server state**: caching, background refetch, invalidation |
| SWR | Simpler server state alternative to React Query |

**Server state vs client state:**
- Server state = data from API (async, can go stale) → React Query
- Client state = UI state (tabs, modals, form state) → useState / Zustand

## Data Fetching Patterns

```jsx
// React Query
const { data, isLoading, error } = useQuery({
  queryKey: ['users', userId],
  queryFn: () => fetchUser(userId),
  staleTime: 60_000,
});
```

Caching, background refetch, pagination, optimistic updates — all built in.

## Hydration (SSR)

```
Server: render React to HTML string → send to browser
Browser: HTML is visible (fast) but not interactive
React client: create VDOM → attach to existing DOM (hydration) → interactive
```

**Hydration mismatch** — server HTML ≠ client render output → React warns + may re-render.

## React Server Components (RSC)

| | Server Component | Client Component |
|-|-----------------|-----------------|
| Runs on | Server only | Server + Client |
| Bundle size | 0 (not sent to client) | Included in bundle |
| State / effects | ❌ No | ✅ Yes |
| DB access | ✅ Direct | ❌ No |
| `"use client"` directive | Not needed | Required |

## Patterns

**Compound Components** — share state implicitly:
```jsx
<Tabs>
  <Tabs.List><Tabs.Tab>Tab 1</Tabs.Tab></Tabs.List>
  <Tabs.Panel>Content 1</Tabs.Panel>
</Tabs>
```

**Render Props** — inject behavior via prop function (largely replaced by hooks)

**HOC (Higher-Order Component)** — `withAuth(Component)` — wraps component with behavior

## See Also

- [[concepts/prompt-engineering]] — LLM-powered React apps
- [[concepts/fastapi]] — FastAPI as React's backend API server
