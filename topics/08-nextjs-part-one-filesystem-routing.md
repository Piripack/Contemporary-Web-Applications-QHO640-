# Topic 8 – Next.js: Part One (Filesystem Routing)

## Overview
This topic introduces **Next.js**, a React framework that supports both static generation and server-side rendering out of the box. You learn the basics of filesystem routing.

## Learning Outcomes
- Understand what Next.js adds to React.
- Create a new Next.js app.
- Use filesystem-based routing.
- Add navigation between pages.

## 1. Creating a Next.js Project
```bash
npx create-next-app@latest my-next-app
cd my-next-app
npm run dev
```
Visit http://localhost:3000.

## 2. File-based Routing
Next.js uses the `pages/` directory to define routes.

```
pages/
  index.js        →  /
  about.js        →  /about
  products/
    index.js      →  /products
    [id].js       →  /products/:id
```

Example page:
```jsx
export default function About() {
  return <h1>About Page</h1>;
}
```

## 3. Navigation
```jsx
import Link from "next/link";

export default function Nav() {
  return (
    <nav>
      <Link href="/">Home</Link> | <Link href="/about">About</Link>
    </nav>
  );
}
```

## 4. Dynamic Routes
File `[id].js` creates a dynamic route.

```jsx
import { useRouter } from "next/router";

export default function Product() {
  const { id } = useRouter().query;
  return <h2>Product ID: {id}</h2>;
}
```

## 5. Pre-rendering and SSR
Next.js pre-renders each page to HTML.
- **Static Generation:** Build-time rendering (fastest).
- **Server-side Rendering:** Render on every request.

Example with SSR:
```jsx
export async function getServerSideProps() {
  return { props: { time: new Date().toISOString() } };
}

export default function TimePage({ time }) {
  return <p>Server time: {time}</p>;
}
```

## Coding Tasks
### Task 8.1 – Setup Project
Create a new Next.js app and run the dev server.

### Task 8.2 – Routing
Add pages `/about`, `/contact`, `/products/[id]`.

### Task 8.3 – Navigation
Build a nav bar using `Link`.

### Task 8.4 – SSR Example
Create a page that shows the current time using `getServerSideProps`.

## Common Mistakes and Fixes
| Problem | Cause | Fix |
|----------|--------|-----|
| 404 error | File name incorrect | Match filename to route |
| Dynamic route undefined | Missing brackets | Use `[param].js` |
| Navigation reloads page | Used `<a>` instead of `<Link>` | Always use `Link` |

## Self-Check Questions
1. What directory defines routes in Next.js?
2. What’s the difference between static and server-side rendering?
3. What hook retrieves dynamic route parameters?

_Last updated: 2025-10-19_
