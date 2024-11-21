# Learning Firebase

## `1. Các set up đầu tiên trong Project `

- `npm i firebase`
- Tạo "firebase" (Folder)>"firebase-config.js" (File) trong Project>src
- Copy config từ trang Firebase, paste vào File firebase-config.js

## `2. firebase-config.js`

```
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";
import { getFirestore } from "firebase/firestore";
// Change your config below
const firebaseConfig = {
  apiKey: "",
  authDomain: "",
  projectId: "",
  storageBucket: "",
  messagingSenderId: "",
  appId: "",
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
// Init services
export const db = getFirestore(app);
export const auth = getAuth(app);

```

## `3. getDocs - get document`

```
import { collection, getDocs } from "firebase/firestore";
import React, { useEffect } from "react";
import { db } from "./firebase-config";

export default function FirebaseApp() {
  const colRef = collection(db, "posts");
  useEffect(() => {
    getDocs(colRef)
      .then((snapshot) => {
        let posts = [];
        snapshot.docs.forEach((doc) => {
          posts.push({
            id: doc.id,
            ...doc.data(),
          });
        });
        setPosts(posts);
        console.log(posts);
      })
      .catch((err) => {
        console.log(err);
      });
  });
}
```

![.](assets/1.PNG)
![.](assets/2.PNG)

## `4. onSnapshot - get document realtime`

```
onSnapshot(colRef, (snapshot) => {
      let posts = [];
      snapshot.docs.forEach((doc) => {
        posts.push({
          id: doc.id,
          ...doc.data(),
        });
      });
      setPosts(posts);
    });
```

## `5. addDoc - add document`

```
addDoc(colRef, {
    title, // data from State
    author, // data from State
    createdAt: serverTimestamp(),
  })
    .then(() => {
      console.log("succcess");
      // reset form
    })
    .catch((err) => {
      console.log(err);
      // reset form
    });
```

## `6. deleteDoc - delete document`

```
const handleRemoveDocument = async (e) => {
    e.preventDefault();
    // Get document ID
    const colRefDelete = doc(db, "posts", postId);
    await deleteDoc(colRefDelete);
  };
```

## `6. updateDoc - update document`

```
 const handleUpdatePost = async (e) => {
    e.preventDefault();
    const colRefUpdate = doc(db, "posts", postId);
    await updateDoc(colRefUpdate, {
      title: "This is a new title from update function",
    });
    console.log("success");
  };
```
