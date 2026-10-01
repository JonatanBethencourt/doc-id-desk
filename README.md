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
3. Go to **Admin → People** and add everyone else with their **name**, **role** (Secretary or
   Administrator), a **username** (filled in from the name; you can change it) and a
   **password** (at least 8 characters; **Suggest one** makes one for you). Give each person
   their username and password privately. Only people on this list can sign in.
4. Go to **Admin → Add a range** and create your categories, for example
   *2026 JWB Document IDs* from `902026254` to `902026899`.

## Everyday use
- **Secretaries**: Request tab → choose category, Regular or MEPS, document type(s), project
  mnemonic and how many → copy the Doc IDs from the pop-up → click **Done**.
- **Exporting records**: on the Records tab, tick the boxes on the left to choose specific
  records (the top box selects everything that matches the filters), then click
  **Export selected**. **Export all** exports every record that matches the filters.
- **MEPS prefix or suffix**: when adding a range, tick **Add a prefix or suffix to the MEPS
  numbers** to add text before or after each MEPS number (for example `S-902026254-E1`).
  Regular numbers are not changed.
- **Administrators**: Admin tab to add ranges or upload spreadsheets, correct or cancel
  assignments, see what's waiting for Done, view utilization reports, manage document
  types and people.

People sign in with the **username and password** an administrator gave them. No email address
is needed. Anyone can change their own password with **Change password** at the top of the page.
If someone forgets it, an administrator clicks **Reset password** next to their name in
**Admin → People**; the old password stops working straight away.

Administrators can also sign in with **Google** or a **sign-in link sent by email**, under
**Other ways to sign in**.

## Reserving Doc IDs and releasing them
Every Doc ID is **Reserved** from the moment it's handed out until someone clicks **Done**.
- To hold Doc IDs for later, click **Keep reserved** in the pop-up and write what they're for
  (for example "Second script, needed in November"). The note appears in Records and exports.
- If reserved Doc IDs end up not being needed, an administrator can **release** them, from
  **Admin → Reserved** or from the record's **Correct, release or cancel** window. A reason is
  required. Released Doc IDs go back to their category and are handed out **first** on the next
  request; the old record stays in the history with the status **Released**. Only release Doc IDs
  that were never pasted into any document. If the category is closed, they go to the Doc ID Bank.
- **Cancelled** is different: a cancelled Doc ID is never reused, because it may have been used
  somewhere. Only Reserved Doc IDs can be released.

## Closing a category and the Doc ID Bank
When a project is over, go to **Admin → Categories**, open **Details and settings** for that
category and click **Close category**.
- Every Doc ID the category never handed out moves to the **Doc ID Bank**.
- Doc IDs already handed out stay with the category with all their records, and are never
  reused (including cancelled ones). Anything still waiting for Done can still be confirmed.
- Secretaries can no longer request Doc IDs from it. Closing can't be undone.

To reuse banked Doc IDs, go to **Admin → Add a range**, choose **The Doc ID Bank** at the top,
pick where to take them from and how many, and add them to a new or existing category (with
a MEPS prefix or suffix if you like). The **Doc ID Bank** tab shows what's there and where
it came from.

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
