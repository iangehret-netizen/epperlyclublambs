# Epperly Club Lambs — Turning on cross-device sync

Until this is done the app works fully on its own: records save on each phone/computer, with no passcode screen.
Once it's connected to Epperly's own Firebase project, every device unlocked with the passcode shares the same records.
Epperly's data is completely separate from Gehret, Petry, Wallen and the other apps.

## 1. Create the Firebase project (one time, ~10 minutes)
1. Go to https://console.firebase.google.com and sign in with the Google account that should own Epperly's data.
2. **Create a project** → name it `epperly-club-lambs` → Google Analytics can be turned off → Create.

## 2. Turn on the passcode login
1. Left menu → **Build → Authentication → Get started**.
2. **Sign-in method** tab → **Email/Password** → Enable → Save.
3. Still on Sign-in method → **Add new provider → Anonymous** → Enable → Save (this is what showman codes use).
4. **Users** tab → **Add user**:
   - Email: any email the Epperly family controls
   - Password: **this is the passcode** everyone types once to unlock a device (6+ characters)

## 3. Turn on the database
1. Left menu → **Build → Firestore Database → Create database** → pick a US location → start in **production mode**.
2. Leave the Rules alone for now — the finished `firestore.rules` comes back with the config.

## 4. Get the web config
1. Gear icon → **Project settings** → scroll to **Your apps** → click the **</>** (Web) icon.
2. Nickname: `Epperly Club Lambs` → Register app (skip Firebase Hosting).
3. Copy the whole `firebaseConfig` block it shows (apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId).

## 5. Allow the website address
Authentication → **Settings** → **Authorized domains** → **Add domain** → `iangehret-netizen.github.io`

## 6. Send to Claude
- The `firebaseConfig` block from step 4
- The passcode email from step 2.4 (not the password)

Claude fills in `sync.js` and `firestore.rules`, bumps the version in `sw.js`, and sends them back.

## 7. Finish
1. Firestore Database → **Rules** tab → select everything, delete it, paste the new `firestore.rules` → **Publish**.
2. Upload the new `sync.js` and `sw.js` to the `epperlyclublambs` repo.
3. On each device, open the app → enter the passcode once → it stays unlocked after that, even offline.

**Note:** Turn sync on *before* entering lots of records, or enter them on one device first — that device's data uploads automatically the first time it unlocks.
