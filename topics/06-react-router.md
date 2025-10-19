# Topic 6 – React Router

## Overview
This topic introduces **React Router**, enabling multiple views and client-side navigation in a single-page app.

## Learning Outcomes
- Install and configure React Router.
- Create multiple routes and navigation links.
- Pass parameters between routes.
- Handle 404 (Not Found) pages.

## 1. Installation
```bash
npm install react-router-dom
```

## 2. Basic Setup
```jsx
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";
import Home from "./Home";
import About from "./About";

export default function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link> | <Link to="/about">About</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

## 3. Route Parameters
```jsx
<Route path="/user/:id" element={<User />} />

function User() {
  const { id } = useParams();
  return <h2>User ID: {id}</h2>;
}
```

## 4. Nested Routes
```jsx
<Route path="/dashboard/*" element={<Dashboard />} />

function Dashboard() {
  return (
    <Routes>
      <Route path="profile" element={<Profile />} />
      <Route path="settings" element={<Settings />} />
    </Routes>
  );
}
```

## 5. Navigation and Redirects
```jsx
import { useNavigate } from "react-router-dom";
const navigate = useNavigate();
<button onClick={() => navigate("/home")}>Go Home</button>;
```

## 6. 404 Page
```jsx
<Route path="*" element={<h1>404 Not Found</h1>} />
```

## Coding Tasks

### Task 6.1 – Setup Router
Create routes for Home, About, and Contact.

### Task 6.2 – Parameters
Add a dynamic route `/product/:id` displaying the ID.

### Task 6.3 – Nested Routes
Create nested dashboard pages.

### Task 6.4 – Redirects
Use `useNavigate` to redirect to home after form submission.

## Common Mistakes and Fixes
| Problem | Cause | Fix |
|----------|--------|-----|
| Blank page | Router not wrapped | Wrap app with `<BrowserRouter>` |
| URL not updating | Missing `Link` or `navigate` | Use router navigation helpers |
| 404 route catching everything | Wrong route order | Place wildcard route last |

## Extra Exercises
1. Build a login redirect using `useNavigate`.
2. Implement navigation highlighting for active links.

## Self-Check Questions
1. What does React Router enable?
2. How do you pass data via URL parameters?
3. How do nested routes differ from normal routes?
4. What hook performs navigation?

## AE1/AE2 Relevance
**AE1:** Shows understanding of single-page application architecture and navigation patterns.
**AE2:** Essential for creating multi-page applications with proper user experience. Demonstrates client-side routing, URL management, and navigation flow - critical components for any modern web application.

_Last updated: 2025-10-19_
