# RADIO REWARDS — PWA v2

A polished bilingual (English/Arabic) PWA starter with three app areas:
- Customer rewards experience
- Cashier QR scanning + stamp workflow
- Admin overview for members, rewards, staff and activity

## Important: what works now vs. what needs setup

**Works in the front-end demo:** screen navigation, EN/AR toggle + RTL, responsive UI, QR camera scanner (browser permission required), manual member ID lookup, demo stamp updates, customer search, dashboard visuals.

**Not production-connected yet:** sign-in, real customer records, admin/staff account management, reward redemption, notification sending, 500m proximity checks, and persistent audit history. Without Firebase environment values, the app runs in DEMO MODE. Demo stamps reset on refresh and are not saved.

The customer QR shown in the design is represented by a member ID in this starter; the actual personal QR generator and account onboarding should be connected to Firebase before launch.

## Run locally

1. Install Node.js LTS.
2. Extract this folder.
3. Open a terminal in the folder and run:
   `npm install`
   `npm run dev`
4. Open the local URL printed by Vite.

## Firebase connection

1. Create a Firebase project.
2. Enable Authentication (Email/Password or phone sign-in, based on the final account flow).
3. Create Firestore.
4. Create a Firebase Web App and copy its config.
5. Copy `.env.example` to `.env` and fill in:
   - `VITE_FIREBASE_API_KEY`
   - `VITE_FIREBASE_AUTH_DOMAIN`
   - `VITE_FIREBASE_PROJECT_ID`
   - `VITE_FIREBASE_APP_ID`
6. Install Functions dependencies inside `functions` and deploy the callable function:
   `npm install`
   `npm install firebase-functions firebase-admin`
   `firebase deploy --only functions`
7. Set Firebase custom claims for each staff account from a trusted admin environment:
   `{ role: "cashier", active: true }` or `{ role: "admin", active: true }`.
8. Deploy Firestore rules only after reviewing your collection schema:
   `firebase deploy --only firestore:rules`

**Never** grant staff stamp-writing access directly from the browser. The provided `addStamp` callable function checks staff claims and writes the stamp + transaction in a server-side transaction. Complete authentication and customer document provisioning before using it with real users.

## QR scanner

The scanner uses `html5-qrcode`. Camera access generally requires HTTPS (localhost is allowed for development) and user permission. Manual ID entry is included as a fallback.

## Notifications / 500m proximity

The 500m radius is a configured product requirement, not implemented in this UI build. Browser PWAs cannot guarantee reliable geofence execution while fully closed, especially on iOS. Implement opt-in, foreground location checks, privacy-conscious cooldowns, and Web Push separately; do not promise guaranteed background proximity alerts.

## Next implementation phase

- Real auth flows and role-aware route protection.
- Firestore customer creation, profile, QR generation and live card.
- Callable reward redemption with atomic transaction + audit.
- Admin CRUD for customers, rewards, employees, reports.
- FCM web push subscription and admin campaign tools.
- Consent-based 500m proximity feature with platform limitations clearly communicated.
