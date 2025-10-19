# Topic 11 – Firebase with React and Next.js

## Overview
This topic combines **Firebase** with **React** and **Next.js** to build full-stack applications. You’ll integrate authentication, Firestore database, and deployment workflows within Next.js.

## Learning Outcomes
- Integrate Firebase Auth and Firestore with React components.
- Use Firebase in server and client contexts in Next.js.
- Protect routes using authentication checks.
- Deploy an integrated Firebase–Next.js application.

## 1. Setting Up Firebase in Next.js
Install Firebase:
```bash
npm install firebase
```

Create `lib/firebase.js`:
```js
import { initializeApp, getApps } from "firebase/app";
import { getAuth } from "firebase/auth";
import { getFirestore } from "firebase/firestore";

const firebaseConfig = {
  apiKey: process.env.NEXT_PUBLIC_FIREBASE_API_KEY,
  authDomain: process.env.NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.NEXT_PUBLIC_FIREBASE_PROJECT_ID,
  storageBucket: process.env.NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.NEXT_PUBLIC_FIREBASE_SENDER_ID,
  appId: process.env.NEXT_PUBLIC_FIREBASE_APP_ID
};

const app = !getApps().length ? initializeApp(firebaseConfig) : getApps()[0];

export const auth = getAuth(app);
export const db = getFirestore(app);
```

## 2. Authentication in React
Use context for global auth state.

**context/AuthContext.js**
```js
import { createContext, useContext, useEffect, useState } from "react";
import { onAuthStateChanged } from "firebase/auth";
import { auth } from "../lib/firebase";

const AuthContext = createContext();

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    const unsubscribe = onAuthStateChanged(auth, setUser);
    return () => unsubscribe();
  }, []);

  return <AuthContext.Provider value={{ user }}>{children}</AuthContext.Provider>;
}

export const useAuth = () => useContext(AuthContext);
```

Wrap `_app.js`:
```jsx
import { AuthProvider } from "../context/AuthContext";
export default function App({ Component, pageProps }) {
  return (
    <AuthProvider>
      <Component {...pageProps} />
    </AuthProvider>
  );
}
```

## 3. Protected Routes
Example in `/pages/profile.js`:
```jsx
import { useAuth } from "../context/AuthContext";
import { useRouter } from "next/router";
import { useEffect } from "react";

export default function Profile() {
  const { user } = useAuth();
  const router = useRouter();

  useEffect(() => {
    if (!user) router.push("/login");
  }, [user]);

  return user ? <h1>Welcome, {user.email}</h1> : <p>Redirecting...</p>;
}
```

## 4. Using Firestore
```js
import { collection, addDoc, getDocs } from "firebase/firestore";
import { db } from "../lib/firebase";

export async function addNote(note) {
  await addDoc(collection(db, "notes"), note);
}

export async function getNotes() {
  const snapshot = await getDocs(collection(db, "notes"));
  return snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
}
```

## 5. Deployment
- Deploy Next.js app via **Vercel**.
- Manage Firebase through **Firebase Console**.
- Update `.env.local` with Firebase config values.

## Coding Tasks
### Task 11.1 – Integrate Firebase SDK
Connect Firebase to Next.js and confirm connection.
### Task 11.2 – Authentication
Implement sign-up, login, and route protection.
### Task 11.3 – Firestore CRUD
Add a note-taking feature using Firestore.
### Task 11.4 – Deployment
Deploy project to Vercel.

## Common Mistakes and Fixes
| Problem | Cause | Fix |
|----------|--------|-----|
| Firebase re-initialisation | Multiple app initialisations | Use `getApps()` guard |
| Route not protected | Missing auth check | Add redirect using `useRouter()` |
| Firebase config undefined | Missing `.env.local` | Add environment variables correctly |

## Self-Check Questions
1. How do you prevent Firebase from being initialised multiple times?
2. How can you share auth state across components?
3. What is the purpose of `.env.local`?
4. What is the best platform to deploy a Next.js + Firebase app?

_Last updated: 2025-10-19_
