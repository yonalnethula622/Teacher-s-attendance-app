# STG Staff Attendance — GitHub Pages starter

This folder contains the first app build for St. Theresa's Gems International Preschool.

## Files
- `index.html` — app UI and first-pass workflows
- `manifest.json` — install name, icon and standalone display
- `icon.png` — home-screen icon
- `sw.js` — basic app-shell service worker
- `firestore.rules` — starter rules to review before deployment

## Important before using real staff data
This is a starter build, not a production-secure attendance backend. The scanner currently uses a client-side Firestore transaction for arrival/departure. Anonymous users can potentially forge attendance if the starter attendance rule is used. Do **not** use the app for real staff attendance until the attendance transition is moved to a trusted Cloud Function and the rules are tightened. The current app also needs Firebase configuration and admin account setup.

## 1. Create a GitHub repository
1. Sign in to GitHub.
2. Create a new repository, e.g. `stg-staff-attendance`.
3. Upload all files from this folder to the repository root.
4. In **Settings → Pages**, choose **Deploy from a branch**, select `main` and `/ (root)`, then Save.
5. Wait for GitHub Pages to publish the HTTPS URL.

## 2. Create Firebase project
1. Open https://console.firebase.google.com/ and create a new project for this app.
2. In Project settings → General → Your apps, add a Web app and copy its Firebase configuration.
3. Open `index.html` and find `const firebaseConfig = { ... }`. Replace all `YOUR_...` values with the exact values Firebase gives you.
4. Find `const ADMIN_EMAIL = "YOUR_ADMIN_EMAIL";` and replace it with the Technical Admin email.
5. In `firestore.rules`, replace `YOUR_ADMIN_EMAIL` with that same email.
6. Enable **Authentication → Sign-in method → Email/Password**.
7. Enable **Authentication → Sign-in method → Anonymous** for the entrance scanner.
8. Create the Technical Admin account from Firebase Authentication. Do not add public sign-up to the app.
9. Create a Firestore database.
10. Enable Firebase Storage if using teacher photo uploads.

## 3. Firestore and Storage
- Publish `firestore.rules` in Firestore → Rules only after reviewing the warning above.
- Storage rules should allow only the verified Technical Admin to upload/view staff photos. Do not make the entire storage bucket public.
- Before production, implement a callable Cloud Function for QR attendance. The function should verify the staff ID, use server time in `Asia/Colombo`, and enforce one arrival and one departure per person per school day. Anonymous clients should not have direct write access to `attendance`.

## 4. Home screen install
- Open the GitHub Pages HTTPS URL on iPhone Safari or Android Chrome.
- iPhone: Share → Add to Home Screen.
- Android: browser menu → Install app / Add to Home screen.
- Camera scanning requires HTTPS and camera permission.

## Features in this starter
- Technical Admin login and device/anonymous login
- Teacher and worker registration with auto IDs
- QR scanner reads the ID you have encoded yourself; it does not create QR codes
- Arrival/departure daily record logic
- Individual PDF attendance export
- Attendance list, delete attendance record, CSV export
- Home-screen manifest and icon

## Notes
- IDs are generated from Firestore counters. Keep the counters and staff records; do not reset them.
- Teacher photo is required; worker type is required.
- The PDF examples are based on data actually saved in Firestore.
