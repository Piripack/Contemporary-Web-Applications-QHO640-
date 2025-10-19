# Topic 3 – Designing Reusable Components

## Overview
This topic explores **component reusability**, **composition**, and **props management** in React. You learn how to refactor large UI sections into smaller, testable, and maintainable components.

## Learning Outcomes
- Understand what makes a React component reusable.
- Pass data and event handlers via props.
- Use **prop types** for validation.
- Create layout and UI components for reuse.
- Understand composition vs inheritance in React.

## 1. What is Reusability?
Reusable components are generic, configurable, and independent of specific data. Instead of copying similar code, you make a single flexible component and pass props to adjust its behaviour.

### Example
```jsx
function Button({ label, onClick, variant = "primary" }) {
  return <button className={variant} onClick={onClick}>{label}</button>;
}
```
Then use:
```jsx
<Button label="Save" onClick={saveData} variant="success" />
<Button label="Cancel" onClick={cancel} variant="danger" />
```

## 2. Composition over Inheritance
React encourages composing components instead of inheriting.

```jsx
function Card({ title, children }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div>{children}</div>
    </div>
  );
}
```
Usage:
```jsx
<Card title="User Profile">
  <UserDetails />
</Card>
```

## 3. Prop Types and Default Props
```jsx
import PropTypes from "prop-types";

function Alert({ message, type }) {
  return <div className={`alert ${type}`}>{message}</div>;
}

Alert.propTypes = {
  message: PropTypes.string.isRequired,
  type: PropTypes.oneOf(["success", "error", "warning"]),
};

Alert.defaultProps = {
  type: "success",
};
```

## 4. Splitting UI into Components
A good rule: if a UI block repeats or has logic that could vary, extract it.

Example:
- App → Dashboard → CardList → Card
- Each level passes only what’s necessary down.

## 5. Passing Callbacks
Children can trigger actions in parents using callbacks passed as props.

```jsx
function SongList({ songs, onSelect }) {
  return songs.map(song => (
    <li key={song.id} onClick={() => onSelect(song)}>{song.title}</li>
  ));
}
```

Parent:
```jsx
<SongList songs={list} onSelect={setSelectedSong} />
```

## Coding Tasks
### Task 3.1 – Create a Button Component
- Props: `label`, `onClick`, `variant`.
- Variants: `primary`, `secondary`, `danger`.
- Use CSS classes.

### Task 3.2 – Compose a Card Component
- Accepts `title` and `children`.
- Used to wrap user profile data.

### Task 3.3 – Use PropTypes
Add validation and default props to your components.

## AE1/AE2 Relevance
**AE1:** Demonstrates component design thinking and reusability principles.
**AE2:** Essential for building scalable applications with consistent UI components. Shows understanding of React architecture patterns and prop validation best practices.

### Task 3.4 – Handle Callbacks
Add a clickable list where each item triggers a parent callback.

## Common Mistakes and Fixes
| Problem | Cause | Fix |
|----------|-------|-----|
| Repeated code | Not refactoring common UI | Extract to new component |
| Child cannot trigger parent change | Callback not passed | Pass handler via props |
| Type error | Missing prop validation | Use PropTypes |

## Extra Exercises
1. Build a `Modal` component with `open` and `onClose` props.
2. Create a reusable `InputField` component for forms.

## Self-Check Questions
1. What are two traits of reusable components?
2. Why use composition over inheritance?
3. How can a child component trigger a parent function?
4. What’s the purpose of `PropTypes`?

_Last updated: 2025-10-19_
