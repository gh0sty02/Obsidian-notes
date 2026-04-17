---
title: ReactJS FAANG-Level Roadmap
tags: [react, javascript, hooks, virtual-dom, state-management, source]
sources: []
last-updated: 2026-04-17
---

# ReactJS FAANG-Level Roadmap

**Source:** `raw/Study Plan/ReactJS.md`, `raw/Interview Questions/React.md`

## Abstract

Comprehensive React roadmap from fundamentals to FAANG-level topics (React Fiber, Concurrent Mode, RSC), state management (Redux/Zustand), data fetching (React Query), and interview-ready deep dives on Virtual DOM, reconciliation, and hydration.

## Key Claims

- React is declarative — you describe what to render; React figures out the minimal DOM updates
- Virtual DOM comparison is O(n) (not O(n³)) due to two heuristics: different types → full rebuild; `key` prop identifies stable elements
- React Fiber enables incremental rendering — work can be paused/resumed/prioritized
- `useEffect` cleanup runs on unmount AND before re-running on dependency change — prevents stale closures
- `React.memo` only prevents re-render if props are shallowly equal — objects/arrays as props still cause re-renders
- Hydration = attaching React event listeners to server-rendered HTML; mismatch causes runtime errors
- React Server Components run only on server — no bundle size, can access DB directly, no state/effects

## Core Concepts

| Concept | Detail |
|---------|--------|
| Virtual DOM | In-memory representation; diffed before touching real DOM |
| Reconciliation | O(n) diff algorithm; `key` prop is critical for lists |
| React Fiber | Concurrent rendering; `startTransition` for low-priority updates |
| Hooks | `useState`, `useEffect`, `useRef`, `useMemo`, `useCallback`, `useReducer` |
| Context API | Avoids prop drilling; triggers re-render for all consumers on change |
| Code Splitting | `React.lazy` + `Suspense` for route-based splitting |

## State Management Options

| Tool | Best For |
|------|---------|
| Context + useReducer | Small-medium apps |
| Redux Toolkit | Large apps needing devtools, middleware |
| Zustand | Lightweight, minimal boilerplate |
| React Query / SWR | Server state, caching, background refetch |

## Connections

- [[concepts/react-fundamentals]] — full concept page
- [[sources/javascript-study-plan]] — JS fundamentals underpin React
