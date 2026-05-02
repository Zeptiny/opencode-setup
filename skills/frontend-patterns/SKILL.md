---
name: frontend-patterns
description: "Use when building React components, Next.js pages, or frontend TypeScript. Examples: React, Next.js, component, hook, useState, useEffect, JSX, frontend, browser, UI logic."
---

# Frontend Patterns

Opinionated React/Next.js patterns for composable components, disciplined state, and performant rendering.

## When to Use

Use this skill when:
- Building or modifying React components, hooks, or client-side TypeScript
- Choosing between useState, useReducer, Context, or Zustand
- Optimizing renders, virtualizing lists, or code-splitting
- Implementing forms, error boundaries, or accessible interactions

Use `frontend-design` instead for pure visual styling decisions.

## Decision Flow

1. **Component structure** — Prefer composition; reach for compound components only when sub-elements share implicit state.
2. **State scope** — Start local, lift to Context for deep trees, add Zustand for global concerns.
3. **Performance** — Profile first; memoize only after measuring a real bottleneck.
4. **Side effects** — Encapsulate in custom hooks; isolate failures with Error Boundaries.

## Examples by Category

### Component Composition

```typescript
export function Card({ children, variant = 'default' }: {
  children: React.ReactNode; variant?: 'default' | 'outlined'
}) {
  return <div className={`card card-${variant}`}>{children}</div>
}
```

### Custom Hook

```typescript
export function useToggle(initial = false) {
  const [value, setValue] = useState(initial)
  const toggle = useCallback(() => setValue(v => !v), [])
  return [value, toggle] as const
}
```

### State Reducer

```typescript
function reducer(state: State, action: Action): State {
  switch (action.type) {
    case 'SET': return { ...state, value: action.payload }
    default: return state
  }
}
```

### Performance Memoization

```typescript
const sorted = useMemo(
  () => items.sort((a, b) => b.score - a.score),
  [items]
)
const handleClick = useCallback((id: string) => select(id), [])
```

## Success Criteria

- Components are composable and prop interfaces are minimal.
- State lives at the lowest common ancestor that needs it.
- Memoization is justified by measurement, not speculation.
- Forms validate before submission and surface accessible errors.
- Async failures are caught by boundaries and do not crash the app.

For the full pattern catalog—including render props, data fetching hooks, debounce, Context+reducer, virtualization, form validation, error boundaries, animations, and accessibility—see [reference.md](./reference.md).
