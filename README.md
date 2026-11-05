# MIXORIA website upgrade starter

This package preserves the original Three.js showroom and adds:
- Working browser cart with quantity controls and local cart persistence
- Checkout form for Cash on Delivery and a bKash Merchant API hand-off
- Customer registration/login with Firebase Email/Password Authentication
- Automatic customer profile documents in Firestore (`customers/{uid}`)
- Customer profile phone/address fields and a My Orders view
- Authenticated server endpoints for saving and retrieving a customer’s own orders
- Server-side bKash integration placeholder (not a live payment integration)

## Important before publishing
1. The cart UI works in the browser, but product prices must be revalidated from Firestore on the server before accepting real orders.
2. `/api/bkash/create-payment` intentionally returns `501` until the current bKash Merchant Tokenized Checkout API is implemented and tested with your merchant credentials.
3. Never put bKash app secret/password or Firebase service-account JSON in `public/`, HTML, or browser JavaScript.
4. Do not use test orders as real customer orders. Test with bKash sandbox/approved test credentials first.

## Firebase project already planned
Project ID: `mixoria-d0fcb`
Web App nickname: `MIXORIA Website`
Previous setup notes: Firestore was created in production mode; the planned collection is `products` with name, price, stock, category, description and image URL. Firebase Storage was intentionally not selected because it required a paid Blaze billing plan; use hosted image URLs for product photos.

## Setup
1. Copy `.env.example` to `.env` for local testing.
2. Add Firebase Admin service-account JSON to `FIREBASE_SERVICE_ACCOUNT_JSON` in the server environment. Do not commit `.env`.
3. The public Firebase Web App config from your project (`mixoria-d0fcb`) is filled in at `public/firebase-config.js`. Email/Password registration and login are connected.
4. In Firebase Console → Firestore → Rules, publish the rules from `firestore.rules`. These allow each signed-in customer to access only their own profile; orders are accessed through the authenticated server API.
5. In Firebase Console → Authentication → Settings → Authorized domains, add your real website domain before production use.
6. Run `npm install`, then `npm start`.
7. Deploy the Node server to a host that supports persistent server processes and HTTPS. A static-only GitHub Pages deployment cannot run the `/api/*` routes.
8. Configure a protected admin role/custom claim before exposing product management.
9. Complete and test bKash Tokenized Checkout server routes using the current merchant documentation and credentials from bKash.

## Current limitations
- Admin panel from the original HTML depends on a special `claude.use()` runtime and is not yet migrated to Firebase Authentication/Firestore.
- The server currently stores orders but does not yet recalculate item prices against Firestore products.
- bKash API create/execute/callback routes are not yet implemented.
- Product catalogue remains the original sample catalogue until Firebase integration is completed.
