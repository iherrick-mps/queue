# Help Queue — Setup Guide

Three files, two free accounts, no code changes needed beyond pasting in your own keys.

## What you're deploying
- `index.html` — the whole app (create rooms, student join form, teacher management, CSV export)
- `firebase-config.js` — the only file you edit, with your Firebase project's keys
- `firestore.rules` — security rules you paste into Firebase (not uploaded to GitHub)

## Part 1 — Create a free Firebase project (~5 minutes)

1. Go to https://console.firebase.google.com and sign in with any Google account. No credit card needed for this.
2. Click **Add project**. Name it anything, e.g. `herrick-help-queue`. You can disable Google Analytics for this project — you don't need it.
3. Once the project loads, click the **web icon (`</>`)** near the top to register a web app. Give it a nickname (e.g. "Help Queue"). You do **not** need to check "Also set up Firebase Hosting" — you're using GitHub Pages instead.
4. Firebase will show you a `firebaseConfig` object with your keys. Copy the whole thing.
5. Open `firebase-config.js` and paste your values in, replacing the placeholders. Save.
6. In the left sidebar, go to **Build → Firestore Database → Create database**. Pick a location close to you (any US region is fine), and choose **Start in production mode**.
7. Once created, click the **Rules** tab. Delete what's there and paste in the contents of `firestore.rules` from this folder. Click **Publish**.
8. Still in Firestore Database, click the **TTL** tab (short for "time to live" — this is what auto-deletes old rooms). Click **Create policy** and add two policies:
   - Collection group: `rooms`, Timestamp field: `expiresAt`
   - Collection group: `entries`, Timestamp field: `expiresAt`

   These tell Firestore to automatically delete any room (and its queue) once its `expiresAt` time passes — 24 hours after creation. It's free and needs no code. Firestore's cleanup itself can lag by up to a day after the expiry moment, but the app already treats a room as "Expired" the instant the 24 hours are up, regardless of when the background cleanup runs.

That's it for Firebase — the free (Spark) tier's limits are far beyond what a classroom queue will ever use.

## Part 2 — Put it on GitHub Pages (~5 minutes)

1. Go to https://github.com and log in (or create a free account).
2. Click **New repository**. Name it something like `help-queue`. Keep it **Public** (Pages is free and simplest on public repos — the Firebase rules already keep your actual data locked down by room code, so this is fine).
3. On the new repo page, click **Add file → Upload files**, and drag in `index.html`, `privacy.html`, `PRIVACY.md`, `queue.ico`, and your edited `firebase-config.js`. Commit the changes.
4. Go to the repo's **Settings → Pages**.
5. Under "Build and deployment," set **Source** to **Deploy from a branch**, branch **main**, folder **/ (root)**. Click **Save**.
6. Wait about a minute, then refresh — GitHub will show you your live URL, something like:
   `https://yourusername.github.io/help-queue/`

That link is your home page. Bookmark it.

## How you'll actually use it

- Open your GitHub Pages link, click **Create New Room**. This now drops you straight into the full teacher view for that room — code, PIN, shareable link, live queue, and CSV export all in one place, no extra click needed.
- Students open the student link, type their name and problem, and appear live in the queue for everyone (including you) to see.
- In teacher view you can: mark someone **Helped** (moves them into a greyed-out "Already Helped" list below the live queue, and keeps them in the CSV record), **Remove** someone entirely (for mistaken or duplicate entries — this does *not* appear in the CSV), and use the **▲ / ▼** arrows to reorder the queue by hand.
- **End Room** stops new requests but keeps the data viewable and exportable (handy at the end of a period). **Reopen Room** undoes that.
- **Delete Room Permanently** immediately and irreversibly wipes the room and its whole queue — download your CSV first if you want a record.
- Every room **automatically deletes itself 24 hours after creation**, whether or not you ever touch it, so nothing lingers in your Firebase project. Make a **new room** each class period (or each day) — it takes about five seconds and keeps each period's data separate and easy to export in the meantime.

## A note on the PIN

Because GitHub Pages only serves static files (no server of its own), the PIN is a practical lock built into the app's interface, not a cryptographic guarantee — similar in spirit to a classroom door that's "closed" versus one that's "locked with a key card." For a middle school help queue, this is the same trust level as a shared Google Sheet, just tidier to use. If you ever want a version with real server-side authentication, that's possible but needs a paid-tier backend function — let me know if that becomes worth it to you.
