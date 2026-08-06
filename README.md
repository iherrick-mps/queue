# Help Queue

A free, no-login digital help queue for the classroom. Students request help with their name and a one-line problem description; the teacher sees a live, ordered queue and can clear students as they're helped. Built for a 1:1 Chromebook classroom, hosted on GitHub Pages, synced live with Firebase.

## How it works

- **Create a room** — generates a unique room code and a 4-digit PIN.
- **Share the room link** — students open it, type their name and problem, and appear instantly in a shared live queue.
- **Manage the room** — enter the PIN to unlock teacher controls: mark students "Helped," remove entries, reorder the queue, end the room, reopen it, or delete it outright.
- **Export the record** — download a CSV of every request in the room (timestamp, name, problem, and when they were helped).
- **Auto-expiry** — every room and its data are automatically deleted 24 hours after creation, no matter what, so nothing accumulates in your Firebase project.

Each room is independent, so a new one can be created per class period or per day without any data mixing between them.

## Tech

- Static site: plain HTML/CSS/JavaScript, no build step or framework
- [Firebase Firestore](https://firebase.google.com/docs/firestore) for live data sync (free Spark tier)
- Hosted on [GitHub Pages](https://pages.github.com/)

## Setup

See [`SETUP.md`](./SETUP.md) for step-by-step instructions to connect your own free Firebase project and deploy to GitHub Pages. In short:

1. Create a Firebase project and Firestore database.
2. Paste your project's config into `firebase-config.js`.
3. Paste the contents of `firestore.rules` into the Firebase console's Rules tab.
4. Push this repo and enable GitHub Pages in **Settings → Pages** (source: `main` branch, root folder).

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire app — room creation, student join form, live queue, teacher controls |
| `firebase-config.js` | Your Firebase project's connection keys (edit this one) |
| `firestore.rules` | Security rules to paste into the Firebase console |
| `SETUP.md` | Full setup walkthrough |

## A note on the PIN

This is a static site with no server of its own, so the PIN is a practical lock built into the app's interface rather than a cryptographic guarantee — comparable to a shared Google Sheet, just tidier to use. It's appropriate for classroom use; it is not intended to protect sensitive data.

## License

Free to use and adapt for your own classroom.