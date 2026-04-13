# Tap Frenzy 🕹️

A multiplayer pub quiz minigame. All teams play simultaneously on their own phones. A host screen (projected) runs the lobby, starts the round, and reveals the live leaderboard.

## Files

| File | Purpose |
|------|---------|
| `host.html` | Open on your laptop, connect to projector. Controls lobby, countdown, leaderboard. |
| `player.html` | Teams scan a QR code to reach this on their phones. Join, wait, play, submit. |

---

## Step 1 — Set Up Firebase (one-time, ~5 minutes)

You need a free Firebase project so all devices can sync in real time.

1. Go to **[console.firebase.google.com](https://console.firebase.google.com)**
2. Click **"Add project"**, give it a name (e.g. `tap-frenzy`), disable Google Analytics (not needed), click Create.
3. In the left sidebar click **Build → Realtime Database**.
4. Click **"Create Database"**.
   - Choose a location close to you (e.g. `europe-west1`).
   - Select **"Start in test mode"** → click Enable.
5. In the left sidebar click the **gear icon → Project settings**.
6. Scroll down to **"Your apps"** → click the **`</>`** (Web) icon.
7. Register the app (any nickname), skip Firebase Hosting.
8. Copy the `firebaseConfig` object that appears — it looks like this:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "tap-frenzy-xxxxx.firebaseapp.com",
  databaseURL: "https://tap-frenzy-xxxxx-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "tap-frenzy-xxxxx",
  storageBucket: "tap-frenzy-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

---

## Step 2 — Paste Your Config Into Both HTML Files

Open **both** `player.html` and `host.html` in a text editor and find this block near the bottom of each file:

```js
const FIREBASE_CONFIG = {
  apiKey:            "REPLACE_WITH_YOUR_API_KEY",
  authDomain:        "REPLACE_WITH_YOUR_PROJECT_ID.firebaseapp.com",
  databaseURL:       "https://REPLACE_WITH_YOUR_PROJECT_ID-default-rtdb.firebaseio.com",
  ...
};
```

Replace every `REPLACE_WITH_...` value with your actual values from Step 1.

> ⚠️  Make sure you update **both files** — they each have their own copy of the config.

---

## Step 3 — Deploy to GitHub Pages (free hosting)

Players need to reach `player.html` from their phones, so the files must be hosted online.

1. Create a new **public** repository on [github.com](https://github.com) (e.g. `tap-frenzy`).
2. Push the two HTML files to the repo:
   ```bash
   cd tap-frenzy
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/tap-frenzy.git
   git push -u origin main
   ```
3. In the repo on GitHub, go to **Settings → Pages**.
4. Under "Branch", select `main` → `/ (root)` → click **Save**.
5. After ~60 seconds, your site is live at:
   - `https://YOUR_USERNAME.github.io/tap-frenzy/host.html`
   - `https://YOUR_USERNAME.github.io/tap-frenzy/player.html`

---

## Step 4 — Night Of The Pub Quiz

1. Open `host.html` on your laptop and connect to the projector/TV.
2. A QR code appears automatically — teams scan it to join.
3. When everyone is in, click **▶ Start Game**.
4. All phones count down together and the 30-second round begins.
5. When time is up, the leaderboard animates in on the projected screen.
6. Click **↺ New Game** to run another round with a fresh room code.

---

## Gameplay

| Target | Points | Colour |
|--------|--------|--------|
| Normal circle | +10 | Cyan / blue |
| Gold bonus | +25 | Gold (appears briefly!) |
| Skull | −5  | Red (appears after ~8s) |
| Empty tap | −2 | — |

- The game gets faster as seconds tick down.
- Scores can't go below 0.
- Sound effects and haptic feedback on every tap.

---

## Troubleshooting

**QR code doesn't appear on host screen**
- Make sure you are online (the QR library loads from a CDN).

**"Room not found" error on phone**
- The room code in the URL doesn't match a live room. Try scanning the QR code again.

**Scores don't appear on leaderboard**
- Check that your Firebase config is correct in both files, especially `databaseURL`.
- In Firebase console → Realtime Database, confirm the database is in **test mode** (rules allow read/write).

**All teams see the lobby but the countdown never fires**
- Check the browser console for Firebase errors.
- Make sure the database URL matches your region (e.g. `europe-west1` not `us-central1`).
