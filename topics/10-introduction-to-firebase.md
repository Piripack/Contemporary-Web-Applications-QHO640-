# Topic 10 – Introduction to Firebase

## Overview
This topic introduces **Firebase**, Google’s backend-as-a-service. You’ll connect Firebase to your React or Next.js app for authentication and database access.

## Learning Outcomes
- Set up a Firebase project.
- Initialise Firebase SDK in React.
- Use Firestore for cloud data storage.
- Authenticate users using Firebase Auth.

## 1. Setting Up Firebase
Visit https://console.firebase.google.com and create a project.

Install Firebase SDK:
```bash
npm install firebase
```

## 2. Initialising Firebase
Create a new file `firebase.js`:
```js
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { getAuth } from "firebase/auth";

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "SENDER_ID",
  appId: "APP_ID"
};

const app = initializeApp(firebaseConfig);
export const db = getFirestore(app);
export const auth = getAuth(app);
```

## 3. Firestore: Reading and Writing
```js
import { collection, addDoc, getDocs } from "firebase/firestore";
import { db } from "./firebase";

async function addSong(song) {
  await addDoc(collection(db, "songs"), song);
}

async function getSongs() {
  const snapshot = await getDocs(collection(db, "songs"));
  return snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
}
```

## 4. Authentication Example
```js
import { createUserWithEmailAndPassword, signInWithEmailAndPassword } from "firebase/auth";
import { auth } from "./firebase";

async function signUp(email, password) {
  await createUserWithEmailAndPassword(auth, email, password);
}

async function signIn(email, password) {
  await signInWithEmailAndPassword(auth, email, password);
}
```

## Coding Tasks
### Task 10.1 – Setup Firebase
Create a Firebase project and connect it to your app.

### Task 10.2 – CRUD with Firestore
Add and fetch records from a `songs` collection.

### Task 10.3 – Authentication
Implement simple sign-up and sign-in using Firebase Auth.

## Common Mistakes and Fixes
| Problem | Cause | Fix |
|----------|--------|-----|
| “Missing Firebase config” | Config not copied | Paste values from console |
| “Permission denied” | Firestore rules restrict access | Allow reads/writes for testing |
| Auth errors | Email format invalid | Validate inputs |

## Self-Check Questions
1. What is Firebase?
2. What services can Firebase provide?
3. Which function initialises Firebase?
4. What are Firestore and Auth used for?

_Last updated: 2025-10-19_
