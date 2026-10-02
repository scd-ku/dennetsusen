# Security

## Firebase Web API key

This project contains a Firebase Web configuration object. The Firebase Web API key is public by design when it is used only for Firebase services. It identifies the Firebase project; it must not be treated as authorization.

Before deployment:

1. In Google Cloud Console > APIs & Services > Credentials, open the Firebase browser key used by this app.
2. Confirm that **API restrictions** are enabled.
3. Allow only the Firebase-related APIs required by this app.
4. Confirm that **Generative Language API** is **not** in the allowlist for this Firebase browser key.
5. Never reuse the Firebase browser key as a Gemini Developer API key.

If the key was unrestricted, or if Generative Language API was allowed, restrict or rotate it before continuing.

## Firestore

The repository cannot enforce Firestore security by hiding the Firebase Web API key.

Review the deployed Firestore Security Rules in Firebase Console. Avoid blanket production rules such as `allow read, write: if true;`.

This repository now includes `firestore.rules`. Each classroom is stored under `rooms/{roomId}`. The anonymous Firebase UID that creates the room is recorded as `ownerUid`; only that UID can classify, reset, or delete data. Other anonymously authenticated participants may create notes, read the room's notes, and increment `likes` by one.

Enable Firebase App Check for Firestore where practical to reduce abuse from unauthorized clients.

## Gemini

The current UI accepts a Gemini API key at runtime. The key is kept in browser memory and is not written to Firestore or exported to CSV, but it is still a client-side credential while in use.

Do not commit a Gemini API key to GitHub and do not distribute a shared Gemini key to students.

For shared classroom use, the recommended architecture is Firebase AI Logic + Firebase App Check (or another server-side proxy). This keeps the Gemini credential on the server side and protects requests from unauthorized clients.

## GitHub secret scanning

GitHub may flag a Firebase Web API key because it matches a generic Google API key pattern.

Do not resolve that alert until you have confirmed the restrictions above. If the key is intentionally public, restricted to Firebase-related APIs, and not usable for Generative Language API, document that fact when resolving the alert.

If the detected value is instead a Gemini API key or another true secret, rotate/revoke it immediately. Removing it from the latest file is not sufficient because it remains in Git history.


## Anonymous classroom ownership

No Google account or manual teacher approval is required. Firebase Anonymous Authentication creates a local anonymous identity automatically.

The room creator's UID is stored as `ownerUid`. This is intentionally bound to the browser profile. Clearing site data, using private browsing, or moving to another device can result in loss of teacher access to an existing room.

Do not weaken `firestore.rules` to work around a lost teacher identity. Create a new room instead and export classroom data before ending a session when retention is important.
