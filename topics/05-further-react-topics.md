# Topic 5 – Further React Topics

## Overview
This topic extends your understanding of React by exploring **keys**, **refs**, **memoisation**, and **performance optimisation**. You also learn when and how to split code into smaller bundles.

## Learning Outcomes
- Use `key` props correctly in lists.
- Access DOM elements using `useRef`.
- Prevent unnecessary re-renders with `React.memo` and `useCallback`.
- Understand lazy loading and code splitting.

## 1. Keys in Lists
Keys help React identify elements that change.

```jsx
<ul>
  {items.map(item => <li key={item.id}>{item.name}</li>)}
</ul>
```
Keys must be **unique** and **consistent** across renders.

## 2. Refs and useRef
Refs access DOM elements or store mutable values without re-rendering.

```jsx
function InputFocus() {
  const inputRef = useRef(null);
  return (
    <div>
      <input ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>Focus</button>
    </div>
  );
}
```

## 3. Memoisation
Use `React.memo`, `useCallback`, and `useMemo` to optimise.

```jsx
const Button = React.memo(({ onClick, label }) => {
  console.log("Rendered:", label);
  return <button onClick={onClick}>{label}</button>;
});
```

`useCallback` ensures stable function references:
```jsx
const handleClick = useCallback(() => setCount(c => c + 1), []);
```

## 4. Code Splitting
Use dynamic imports or React.lazy to load code only when needed.

```jsx
const Profile = React.lazy(() => import('./Profile'));
<Suspense fallback={<div>Loading...</div>}>
  <Profile />
</Suspense>
```

## 5. Error Boundaries
Catch component-level errors gracefully.

```jsx
class ErrorBoundary extends React.Component {
  constructor() { super(); this.state = { hasError: false }; }
  static getDerivedStateFromError() { return { hasError: true }; }
  render() {
    if (this.state.hasError) return <h2>Something went wrong.</h2>;
    return this.props.children;
  }
}
```

## Coding Tasks

### Task 5.1 – useRef Example
Build an input box that focuses on button click.

### Task 5.2 – Memoisation
Optimise a child component using `React.memo`.

### Task 5.3 – Lazy Loading
Load a Profile component dynamically with a fallback.

### Task 5.4 – Error Boundary
Wrap a child component in an Error Boundary to catch runtime errors.

## Common Mistakes and Fixes
| Problem | Cause | Fix |
|----------|--------|-----|
| List items flicker | Missing or unstable keys | Use unique `key` |
| Refs not working | Assigned in loops | Attach directly to DOM |
| Excessive re-renders | Functions recreated every render | Use `useCallback` |

## Extra Exercises
1. Lazy-load two routes using React Router.
2. Add `useMemo` to cache expensive computations.

## Self-Check Questions
1. Why must list items have keys?
2. What’s the difference between `useRef` and `useState`?
3. When should you use `React.memo`?
4. What does `React.lazy` achieve?

_Last updated: 2025-10-19_
