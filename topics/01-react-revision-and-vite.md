# Topic 1 – React Revision and Vite

## Overview
This topic revises basic React concepts and introduces **Vite** as a modern development and bundling tool for React applications.
It provides a faster and simpler alternative to older tools like Webpack by supporting hot module reloading (HMR) and automatic bundling through **Rollup**.

## Learning Outcomes
By the end of this topic, you should be able to:
- Explain why JSX and bundlers are required for modern React development.
- Set up and serve a React app using Vite.
- Use the Vite React plugin to enable JSX and fast refresh.
- Build and serve production-ready bundles.
- Understand the link between development setup and AE1/AE2 deliverables.

## 1. React Recap
React is a JavaScript library for building user interfaces using **components**—reusable blocks that handle their own state.

### Example
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<h1>Hello World!</h1>);
```

### Why this code does not run directly in the browser
1. **React not installed** – Browsers can’t fetch npm modules directly.
2. **Imports not recognised** – `import React from 'react'` requires bundling.
3. **JSX syntax unsupported** – Must be transpiled into JavaScript (via **Babel**).

## 2. Using Vite
Vite simplifies both development and production processes.

**Vite has two parts:**
- **Development server:** runs on port 5173, supports imports and live reloading.
- **Bundler:** creates production-ready builds using Rollup.

### Installation and Setup
```bash
npm install vite
```

Create `index.html` in the project root:
```html
<!DOCTYPE html>
<html>
  <body>
    <div id="root"></div>
    <script type="module" src="main.jsx"></script>
  </body>
</html>
```

Run the dev server:
```bash
npx vite dev
```
Visit http://localhost:5173 and edit files to see live updates.

## 3. Adding React to Vite
Install React and the React plugin:
```bash
npm install @vitejs/plugin-react react react-dom
```

Create **vite.config.mjs**:
```js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()]
});
```
Vite now recognises JSX and supports React fast refresh.

## 4. Development vs Production
- **Development:** Vite serves files directly with instant updates.
- **Production:** Run `npx vite build` to generate optimised bundles in `/dist`.

Example configuration for custom output folder:
```js
build: {
  outDir: 'built'
}
```

## Coding Tasks

### Coding Task 1.1 – Serve Static HTML
Use Vite to serve a simple HTML page.

**Steps**
1. Install Vite.
2. Create a plain `index.html` with “Hello World.”
3. Run `npx vite dev`.
4. Edit the file while running; the browser reloads automatically.

### Coding Task 1.2 – Serve React Page
Enhance the HTML page using React.

**Steps**
1. Install the Vite React plugin.
2. Add a `src/` folder with `main.jsx` using the Hello World example.
3. Link the JS file as a module in `index.html`.
4. Observe automatic updates in the browser.

### Coding Task 1.3 – React Counter Component
Create a React component that:
- Increments a counter when a button is clicked.
- Resets the counter to 0 on another button press.

Import and use this component from `main.jsx`.

### Coding Task 1.4 – Build for Production
Build your app for production and serve it with Express.

**Steps**
1. Run `npx vite build`.
2. Set up an Express server to serve static files from `/dist`.
3. Visit the app in a browser via the Express route.

### Coding Task 1.5 – HitTastic! Starter
Use the HitTastic React starter.

**Extension Goals**
- Filter songs by artist and display results using `map()`.
- Store results and theme mode (light/dark) in state.
- Apply dynamic styles based on theme.
- Test using the Vite server, then build and serve with Express.

## Common Mistakes and Fixes

| Problem | Cause | Fix |
| -------- | ------ | --- |
| “Cannot find module ‘react’” | React not installed | Run `npm install react react-dom` |
| Page not updating | Dev server stopped | Run `npx vite dev` |
| “Plugin not found” | Missing Vite React plugin | Install `@vitejs/plugin-react` |
| Blank screen | Incorrect `root` div ID | Ensure `id="root"` matches |

## Extra Exercises

### Exercise 1: Static HTML
Serve a simple “Hello Vite!” page.

<details>
<summary>Solution</summary>

1. Create `index.html` in the root.
2. Run `npx vite dev`.
3. Edit and save to see HMR in action.

</details>

### Exercise 2: React Component
Convert the HTML page into a React app.

<details>
<summary>Solution</summary>

1. Install React and plugin.
2. Create `main.jsx` with:
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<h1>Hello React with Vite!</h1>);
```
3. Configure Vite using `vite.config.mjs`.

</details>

## Self-Check Questions

1. Why can’t browsers interpret JSX?
   <details><summary>Answer</summary>It must be transpiled to JavaScript using Babel.</details>

2. What are Vite’s two key functions?
   <details><summary>Answer</summary>Development server with HMR and production bundler using Rollup.</details>

3. How do you start the dev server?
   <details><summary>Answer</summary>`npx vite dev`</details>

4. What is `vite.config.mjs` for?
   <details><summary>Answer</summary>It specifies plugins and configuration for Vite.</details>

5. Default Vite dev server port?
   <details><summary>Answer</summary>5173</details>

## Further Reading
- Vite Documentation
- React Official Docs
- HitTastic React Starter Repository
- Rollup Documentation
- Serving Static Files with Express

## AE1/AE2 Relevance
**AE1:** Development environment and setup decisions.
**AE2:** Foundation for implementing, testing, and deploying with Vite.

_Last updated: 2025-10-19_
