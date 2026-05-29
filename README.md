# Castle Click — Upload Tracker

A lightweight internal web app to track vendor uploads to Castle Click.
Hosted on **Netlify**, database powered by **Firebase Firestore**.

---

## Setup (15 minutes)

### Step 1 — Create a Firebase Project

1. Go to https://console.firebase.google.com
2. Click **Add project** → give it a name (e.g. `castle-click-tracker`)
3. Disable Google Analytics if you don't need it → **Create project**

### Step 2 — Create a Firestore Database

1. In the left sidebar click **Firestore Database** → **Create database**
2. Choose **Start in test mode** (you can lock it down later)
3. Pick a region close to you (e.g. `us-central`) → **Enable**

### Step 3 — Register a Web App & Get Config

1. In Project Overview click the **</>** (web) icon → give it a name
2. Copy the `firebaseConfig` object that appears — it looks like:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

3. Open `index.html` and find the section marked `// ── REPLACE THESE VALUES ──`
4. Paste your values in
5. Delete the `<div class="config-banner">` block at the top of `<main>`

### Step 4 — Deploy to Netlify

1. Push this folder to a **GitHub repository**
2. Go to https://app.netlify.com → **Add new site → Import from Git**
3. Connect your GitHub account → select the repo
4. Build settings:
   - Build command: *(leave blank)*
   - Publish directory: `.`
5. Click **Deploy site**

Netlify will give you a URL like `https://your-site-name.netlify.app` — share that with your team!

### Step 5 — (Optional) Custom Domain

In Netlify → **Domain management** → add your own domain if you have one.

---

## Security Note

The Firebase config values (API key etc.) are safe to expose in client-side code —
Firebase's security is controlled by **Firestore Security Rules**, not by keeping
the config secret.

Once you're ready to lock down access (so only your team can use it), update your
Firestore rules in the Firebase Console under **Firestore → Rules**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /uploads/{doc} {
      allow read, write: if request.auth != null;
    }
  }
}
```

This requires users to be logged in. Ask if you want to add Firebase Authentication.

---

## Features

- Add upload records with vendor name, dates, and success status
- Search/filter by vendor name
- Delete records
- Live stats (total / success / failed)
- Fully responsive
