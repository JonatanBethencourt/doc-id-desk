# Doc ID Desk

A web app for handing out Document IDs (Doc IDs) from ranges, in Regular or MEPS format,
with a complete audit trail. It runs as a static page on **GitHub Pages** and stores its
data in **Firebase** (Google's hosted database and sign-in). There is no server to run.

## What's in this repository

| File | What it is |
|---|---|
| `index.html` | The whole app |
| `firebase-config.js` | Your Firebase project's settings (you fill this in) |
| `firestore.rules` | Security rules for the database (you paste these into Firebase) |
| `README.md` | These instructions |

## Setup (about 20 minutes, once)

### 1. Put the files on GitHub Pages
1. Create a new repository on GitHub (for example `docid-desk`).
2. Upload the four files: **Add file → Upload files**, drag them in, then **Commit changes**.
3. Go to **Settings → Pages**. Under *Build and deployment*, set **Source** to
   *Deploy from a branch*, choose branch **main** and folder **/ (root)**, then **Save**.
4. After a minute your site is live at `https://YOUR-USERNAME.github.io/docid-desk/`.
   It will say it isn't connected to a database yet. That's expected.

### 2. Create the Firebase project
1. Go to <https://console.firebase.google.com> and click **Create a project**
   (Google Analytics is not needed).
2. **Build → Firestore Database → Create database**. Pick the location closest to you and
   start in **production mode**.
3. **Build → Authentication → Get started**, then on the **Sign-in method** tab enable:
   - **Google**
   - **Email/Password**, and inside it turn on **Email link (passwordless sign-in)**
4. Still in Authentication, open **Settings → Authorized domains → Add domain** and add
   `YOUR-USERNAME.github.io`.
5. Click the gear icon **→ Project settings**. Under *Your apps*, click the web icon
   **`</>`**, give the app a nickname, and register it (skip Firebase Hosting).
   Firebase shows a `firebaseConfig` block. Keep that page open.

### 3. Connect the app to Firebase
1. On GitHub, open `firebase-config.js` and click the pencil icon to edit it.
2. Replace the placeholder values with the ones from your `firebaseConfig` block
   (`apiKey`, `authDomain`, `projectId`, `storageBucket`, `messagingSenderId`, `appId`).
3. Commit the change.

These values are safe to be public; they only identify your project. Protection comes
from the security rules in the next step.

### 4. Publish the security rules
1. In Firebase, go to **Firestore Database → Rules**.
2. Replace everything there with the contents of `firestore.rules`, then click **Publish**.

### 5. Become the administrator
1. Open your GitHub Pages address and sign in.
2. You'll see **Set up Doc ID Desk**. Click **Make me the administrator**.
   Only the very first person can do this, so do it straight away after publishing.
3. Go to **Admin → People** and add everyone else by email address, as **Secretary** or
   **Administrator**. Only people on this list can sign in.
4. Go to **Admin → Add a range** and create your categories, for example
   *2026 JWB Document IDs* from `902026254` to `902026899`.

## Everyday use
- **Secretaries**: Request tab → choose category, Regular or MEPS, document type(s), project
  mnemonic and how many → copy the Doc IDs from the pop-up → click **Done**.
- **Administrators**: Admin tab to add ranges or upload spreadsheets, correct or cancel
  assignments, see what's waiting for Done, view utilization reports, manage document
  types and people.

People sign in with **Google** or with a **sign-in link sent by email** (works for any email
address, including work addresses).

## How Doc IDs are protected
- Every hand-out is a database **transaction**. If two people request at the same moment,
  Firestore retries one of them, so the same Doc ID can never go to two requests.
- The security rules only let each category's position move **forward**, so a Doc ID can't
  be handed out a second time, even by a modified copy of the page.
- Records are never deleted. Cancelled Doc IDs stay cancelled and are never reused.
- MEPS numbers are generated in the *MEPS Bookman Universal* characters
  (1–9 = U+F734–U+F73C, 0 = U+F73D), identical to the MEPS column in the original spreadsheets.

## Cost
A team of this size fits comfortably in Firebase's free **Spark** plan.

## Updating the app later
Replace `index.html` in the repository with a new version. Your data lives in Firebase and
is not affected.
