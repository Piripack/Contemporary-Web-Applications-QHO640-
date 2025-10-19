# Topic 4 – React Hooks and Context

## Overview
You will learn how to use React **Hooks** (`useState`, `useEffect`, `useContext`, `useReducer`) and **Context API** to solve prop-drilling problems introduced in Topic 3.

## Learning Outcomes
- Use `useState` for component state.
- Use `useEffect` for side effects.
- Use `useContext` and `createContext` to share global data.
- Use `useReducer` for complex state logic.

## 1. Why Hooks?
Hooks simplify state management and lifecycle handling in functional components.

```jsx
const [count, setCount] = useState(0);
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);
```

## 2. Context for Global State
Avoid passing props through multiple layers using **Context**.

```jsx
const ThemeContext = createContext();

function App() {
  const [theme, setTheme] = useState("light");
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  const { theme, setTheme } = useContext(ThemeContext);
  return <button onClick={() => setTheme(theme === "light" ? "dark" : "light")}>Toggle Theme</button>;
}
```

## 3. useReducer for Complex Logic
```jsx
function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      return state;
  }
}

const [state, dispatch] = useReducer(reducer, { count: 0 });
```

## 4. Combining Hooks
Hooks can be combined:
```jsx
useEffect(() => {
  console.log("Theme changed:", theme);
}, [theme]);
```

## Coding Tasks
### Task 4.1 – Counter with useState
Build a counter with increment and reset buttons.

### Task 4.2 – Theme Context
Use Context to share a theme between components.

### Task 4.3 – useReducer Example
Manage counter logic with reducer instead of multiple states.

## Common Mistakes and Fixes
| Problem | Cause | Fix |
|---|---|---|
| “Invalid hook call” | Hook used in nested function | Use hooks only in top-level React function |
| No re-render | State mutated directly | Use setter (`setState`) |
| Context undefined | Used outside provider | Wrap in `Context.Provider` |

## Extra Exercises
1. Combine `useContext` and `useReducer` to manage user authentication state.
2. Create a `ThemeSwitcher` that toggles global theme and saves preference in `localStorage`.

## Self-Check Questions
1. Why are hooks needed?
2. What problem does Context solve?

## AE1/AE2 Relevance
**AE1:** Demonstrates advanced React patterns and state management architecture decisions.
**AE2:** Critical for building complex applications with proper state management. Shows understanding of React Context API, custom hooks, and modern React patterns essential for scalable web applications.

_Last updated: 2025-10-19_
