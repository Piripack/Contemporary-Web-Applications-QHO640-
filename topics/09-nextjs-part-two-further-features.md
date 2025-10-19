# Topic 9 – Next.js: Part Two (Further Features)

## Overview
This topic builds on the previous Next.js lesson by introducing **API routes**, **data fetching methods**, **static generation**, and **environment configuration**. You also learn to deploy your Next.js app.

## Learning Outcomes
- Create API routes in Next.js.
- Use `getStaticProps` and `getStaticPaths`.
- Configure environment variables.
- Prepare a Next.js app for deployment.

## 1. API Routes
Next.js allows creating backend routes inside the same project.

```
pages/api/hello.js
```
```js
export default function handler(req, res) {
  res.status(200).json({ message: "Hello from API route!" });
}
```
Visit `/api/hello` in the browser.

## 2. Data Fetching Methods
### `getStaticProps` – Build-time data
```jsx
export async function getStaticProps() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts');
  const posts = await res.json();
  return { props: { posts } };
}
```
Use when data is mostly static.

### `getServerSideProps` – Run-time data
```jsx
export async function getServerSideProps() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts/1');
  const post = await res.json();
  return { props: { post } };
}
```

### `getStaticPaths` – Dynamic routes
```jsx
export async function getStaticPaths() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts');
  const posts = await res.json();
  const paths = posts.map(p => ({ params: { id: p.id.toString() } }));
  return { paths, fallback: false };
}
```

## 3. Environment Variables
Create `.env.local`:
```
NEXT_PUBLIC_API_URL=https://api.example.com
```
Access it via:
```js
process.env.NEXT_PUBLIC_API_URL
```

## 4. Deployment
Run the production build:
```bash
npm run build
npm start
```
Then deploy to platforms like **Vercel** (Next.js creator) or **Netlify**.

## Coding Tasks
### Task 9.1 – Create API Route
Make an `/api/time` endpoint that returns the current server time.

### Task 9.2 – Static Generation
Fetch and display posts using `getStaticProps`.

### Task 9.3 – Dynamic Paths
Generate pages for individual posts using `getStaticPaths`.

### Task 9.4 – Environment Variables
Read an API base URL from `.env.local` and display it on a page.

## Common Mistakes and Fixes
| Problem | Cause | Fix |
|----------|--------|-----|
| API route 404 | Wrong file location | Must be under `pages/api/` |
| `process.env` undefined | Variable not prefixed with `NEXT_PUBLIC_` | Add prefix for client-side |
| Build fails | Network fetch in static build | Ensure fetch works at build time |

## Self-Check Questions
1. What directory holds API routes?
2. When use `getStaticProps` vs `getServerSideProps`?
3. Why prefix environment variables with `NEXT_PUBLIC_`?
4. Which command builds the production bundle?

_Last updated: 2025-10-19_
