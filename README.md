# KFS SQUAD Event Tickets

Static event and ticket-management website backed by Firebase Authentication
and Cloud Firestore.

## Files

- `index.html`: public event page and checkout.
- `admin.html`: authenticated payment review and gate scanner.
- `firestore.rules`: required database access controls.

## Security setup

Deploy the included Firestore rules before accepting real bookings:

```sh
firebase use kfs-squad-event
firebase deploy --only firestore:rules
```

Only Firebase Authentication accounts belonging to the event team should exist
in this Firebase project. The current rules treat every authenticated account as
an administrator.

The checkout stores all tickets and payment evidence in one Firestore
transaction. The capacity check, ticket creation, screenshot creation, and
`sold` update therefore succeed or fail together.

## Remaining production hardening

The public client must be able to increment `event/main.sold` to complete the
current static checkout. A malicious client could still inflate that counter
without creating tickets. The final production architecture should move
checkout into a trusted Cloud Function or server endpoint and deny all public
writes to `event/main`.

Payment screenshots should also move to Firebase Storage instead of Firestore
before booking volume grows.
