# Topic 2 – Effects, Server Communication, and Lifting State Up

## Overview
You replace hard-coded data with real API calls. You avoid CORS and port issues in development by running Vite in middleware mode inside Express using **vite-express**. You fetch on load with **useEffect** and lift state to the right parent.

## Learning Outcomes
- Integrate Vite with Express using `vite-express`.
- Call REST endpoints from React and manage results in state.
- Control effect timing with the dependency array.
- Lift state so siblings share data without prop chaos.

## 1. The development problem
Two servers cause friction:
- Port coupling: front-end on 5173 must call `http://localhost:3000`.
- CORS errors from different origins.

## 2. The solution: vite-express
Install and run Vite as Express middleware.

```bash
npm install vite-express
```

**server.mjs**
```js
import express from 'express';
import ViteExpress from 'vite-express';

const app = express();

ViteExpress.listen(app, 3000, () => {
  console.log("Express + Vite at http://localhost:3000");
});
```

Now browse at `http://localhost:3000`. HMR still works.

### Production
```bash
npx vite build
NODE_ENV=production node server.mjs
```
With `NODE_ENV=production`, vite-express serves `/dist` automatically.

## 3. Backend routes you need
Expose two endpoints on your Express server:
- `GET /api/songs/all` → returns all songs as JSON.
- `GET /api/songs/artist/:artistName` → returns filtered songs.

## 4. Front-end changes
On search:
- `fetch('/api/songs/artist/Oasis')`
- Parse JSON.
- Save to state. Remove hard-coded arrays.

## 5. Fetching on load with effects
Effects run after render. Use the dependency array to control when.

- `useEffect(fn)` → every render. Not for network calls.
- `useEffect(fn, [])` → once on first render. Good for initial fetch.
- `useEffect(fn, [a,b])` → on first render and when `a` or `b` change.

Pattern for an async fetch inside an effect:
```jsx
import { useEffect, useState } from 'react';

export default function Songs() {
  const [songs, setSongs] = useState([]);

  useEffect(() => {
    let cancelled = false;
    (async () => {
      const res = await fetch('/api/songs/all');
      const data = await res.json();
      if (!cancelled) setSongs(data);
    })();
    return () => { cancelled = true; };
  }, []);

  return <ul>{songs.map(s => <li key={s.id}>{s.title}</li>)}</ul>;
}
```

## 6. Lifting state up
Keep shared state in the **nearest common parent**.

- Search input updates `artist` in parent.
- Parent fetches results and passes them down as props to results list.
- Avoid deep prop-drilling where context or hooks are better (you will cover in Topic 4).

## Coding Tasks

### Coding Task 2.1 – Run Express with Vite
- Install `vite-express`.
- Create `server.mjs` as above.
- Start: `node server.mjs`.
- Verify HMR at `http://localhost:3000`.

### Coding Task 2.2 – Build the API and wire the client
- Implement `/api/songs/all` and `/api/songs/artist/:artistName`.
- From React, fetch on search click and store the array in state.
- Delete the hard-coded data.

### Coding Task 2.3 – Fetch on load with an effect
- Add `useEffect(..., [])` in the component that displays all songs.
- Fetch `/api/songs/all` and render the list.
- Guard with a cancellation flag in cleanup.

## Common Mistakes and Fixes
| Problem | Cause | Fix |
|---|---|---|
| CORS error | Front-end calls a different origin | Use vite-express so app and API share origin |
| Infinite requests | `useEffect` without deps | Use `[]` or the correct dependency list |
| Stale results | Fetch in child where parent props change | Lift state and fetch in the parent |
| HMR not working | Browsing 5173 while server on 3000 | Use `http://localhost:3000` during dev |

## Extra Exercises

### Exercise A: Debounced search
Implement a debounced input that fetches only after 300 ms of no typing.

<details>
<summary>Solution</summary>

Use a `useEffect` watching `artist`. Inside, `setTimeout` to perform fetch after 300 ms. Clear the timeout in the cleanup. Update results state with the response.
</details>

### Exercise B: Error and loading states
Add `loading` and `error` state to the songs view.

<details>
<summary>Solution</summary>

Before fetch: `setLoading(true)`. On success: `setSongs(data); setLoading(false)`. On failure: `setError(msg); setLoading(false)`. Render conditional UI for each state.
</details>

## Self-Check Questions
1. Why does vite-express remove CORS issues?
   <details><summary>Answer</summary>Front-end and API share the same origin and port via one Express server.</details>
2. When should an effect run only once?
   <details><summary>Answer</summary>On initial load when you pass an empty dependency array.</details>
3. Where should shared search results state live?
   <details><summary>Answer</summary>In the nearest common parent of the input and results components.</details>
4. What production command serves the bundle?
   <details><summary>Answer</summary>`NODE_ENV=production node server.mjs` after `npx vite build`.</details>

## Further Reading
- vite-express
- React useEffect
- Same-Origin Policy and CORS

## AE1/AE2 Relevance
**AE1:** Shows design decisions about state ownership and data flow.
**AE2:** Provides a development + production server model and robust data fetching.

_Last updated: 2025-10-19_
