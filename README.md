# supabase-db-with-react-project
This project gives a general guideline on how you can use an online-based database platform-SUPABASE, for your React project. It can help in Signing up new users, Logging in with email + password, Logging out, and doing Session tracking.

**repo layout**:

```
supabase-react-auth/
│
├── package.json
├── README.md
├── public/
│   └── index.html
└── src/
    ├── index.js
    ├── App.js
    ├── supabaseClient.js
    └── components/
        └── Auth.js
```

---

## 1. `package.json`

```json
{
  "name": "supabase-react-auth",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "@supabase/supabase-js": "^2.45.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build"
  }
}
```

---

## 2. `public/index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Supabase React Auth</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

---

## 3. `src/index.js`

```javascript
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

---

## 4. `src/supabaseClient.js`

```javascript
import { createClient } from "@supabase/supabase-js";

// Replace these with your Supabase project details
const supabaseUrl = "https://YOUR_PROJECT_ID.supabase.co";
const supabaseAnonKey = "YOUR_ANON_KEY";

export const supabase = createClient(supabaseUrl, supabaseAnonKey);
```

---

## 5. `src/App.js`

```javascript
import React, { useState, useEffect } from "react";
import { supabase } from "./supabaseClient";
import Auth from "./components/Auth";

export default function App() {
  const [session, setSession] = useState(null);

  useEffect(() => {
    supabase.auth.getSession().then(({ data }) => {
      setSession(data.session);
    });

    const { data: listener } = supabase.auth.onAuthStateChange((_event, session) => {
      setSession(session);
    });

    return () => listener.subscription.unsubscribe();
  }, []);

  return (
    <div style={{ padding: "2rem", fontFamily: "Arial" }}>
      <h1>📚 Supabase React Auth Example</h1>
      {!session ? (
        <Auth />
      ) : (
        <div>
          <p>Signed in as {session.user.email}</p>
          <button onClick={() => supabase.auth.signOut()}>Logout</button>
        </div>
      )}
    </div>
  );
}
```

---

## 6. `src/components/Auth.js`

```javascript
import React, { useState } from "react";
import { supabase } from "../supabaseClient";

export default function Auth() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const signUp = async () => {
    const { error } = await supabase.auth.signUp({ email, password });
    if (error) alert(error.message);
    else alert("Check your email for confirmation link!");
  };

  const signIn = async () => {
    const { error } = await supabase.auth.signInWithPassword({ email, password });
    if (error) alert(error.message);
  };

  return (
    <div style={{ maxWidth: "300px", margin: "auto" }}>
      <h2>Login / Sign Up</h2>
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        style={{ width: "100%", marginBottom: "10px" }}
      />
      <input
        type="password"
        placeholder="Password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        style={{ width: "100%", marginBottom: "10px" }}
      />
      <button onClick={signIn} style={{ marginRight: "10px" }}>Login</button>
      <button onClick={signUp}>Sign Up</button>
    </div>
  );
}
```

---

## 7. `README.md`

````markdown
# 📚 Supabase React Auth Example

A minimal React project showing how to use Supabase authentication with email + password.

---

## 🚀 Getting Started

1. Clone this repo (or copy files into a new folder):
   ```bash
   git clone https://github.com/YOUR_USERNAME/supabase-react-auth.git
   cd supabase-react-auth
````

2. Install dependencies:

   ```bash
   npm install
   ```

3. Add your Supabase project URL + anon key in:

   * `src/supabaseClient.js`

4. Run the app:

   ```bash
   npm start
   ```

---

## 🛠 Features

* Sign up new users
* Log in with email + password
* Log out
* Session tracking

```

