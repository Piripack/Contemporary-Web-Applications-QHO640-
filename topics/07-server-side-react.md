# Topic 7 – Introduction to Server-side React

## Overview
This topic introduces **server-side rendering (SSR)** using React. You will understand how rendering React components on the server improves SEO, load time, and perceived performance.

## Learning Outcomes
- Explain client-side rendering vs server-side rendering.
- Configure an Express server to render React components.
- Use `renderToString()` from `react-dom/server`.
- Understand hydration and how the client reuses server-rendered HTML.

## 1. What is SSR?
Client-side React builds HTML in the browser. SSR builds HTML on the server first, sending complete content to the browser for faster first paint.

### Diagram
Server renders → Browser displays → React hydrates to attach interactivity.

## 2. Setting Up Server-side Rendering
Install dependencies:
```bash
npm install express react react-dom
```

**server.mjs**
```js
import express from "express";
import { renderToString } from "react-dom/server";
import React from "react";
import App from "./src/App.js";

const app = express();

app.use(express.static("dist"));

app.get("*", (req, res) => {
  const html = renderToString(<App />);
  res.send(`
    <!DOCTYPE html>
    <html>
      <head><title>SSR React</title></head>
      <body><div id="root">${html}</div><script src="/client.js"></script></body>
    </html>`);
});

app.listen(3000);
```

## 3. Hydration
The client attaches React events to the static HTML.

```jsx
import { hydrateRoot } from "react-dom/client";
import App from "./App.jsx";

hydrateRoot(document.getElementById("root"), <App />);
```

## 4. Benefits of SSR
- Faster first contentful paint.
- Better SEO (content visible to crawlers).
- Can prefetch data server-side.

## Coding Tasks
### Task 7.1 – Server Setup
Create an Express server using `renderToString`.

### Task 7.2 – Hydration
Hydrate the same HTML client-side using `hydrateRoot`.

### Task 7.3 – Add Routing
Integrate React Router into SSR setup.

## Common Mistakes and Fixes
| Problem | Cause | Fix |
|----------|--------|-----|
| Blank page | Missing hydration script | Ensure same component tree is used |
| Mismatch error | Different markup server vs client | Use identical props/data |
| SEO not improving | Content still loaded client-side | Move data fetching to server |

## Self-Check Questions
1. What’s the main advantage of SSR?
2. What method renders React to HTML on the server?
3. What’s hydration?

_Last updated: 2025-10-19_
