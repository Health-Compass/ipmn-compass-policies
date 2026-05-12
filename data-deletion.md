# IPMN Compass — Account & Data Deletion

**Effective Date:** 2026-05-12 (Phase 0 — pre-MVP)

This page describes how to request deletion of your IPMN Compass
account and any associated data. Required by Google Play (Data Safety)
and Apple App Store (App Privacy) policies; mirrors the rights enumerated
in [`privacy-policy.md`](privacy-policy.md).

---

## How to request deletion

**Email:** `jason.castellanos@gmail.com`

In your message, please include:

- The email address you used to register your IPMN Compass account.
- A brief statement requesting deletion (e.g., "Please delete my
  IPMN Compass account and all associated data.").

You do not need to provide a reason.

We will acknowledge your request within 5 business days and complete
the deletion within 30 days of acknowledgment. Some legal-retention
obligations may require us to keep minimal records of the request
itself (timestamps, the fact of deletion) — never the deleted content.

## What gets deleted

When you request account deletion, we delete:

- **Firebase Authentication record** — your email + auth credentials.
  Removes your ability to sign in.
- **Firestore user document and sub-collections** — any patient-entered
  cyst features, reported symptoms, surveillance imaging dates,
  preferences, and acceptance records.
- **Per-uid device cache** — local SecureStore and AsyncStorage entries
  scoped to your account (cleared the next time you launch the app).
- **Sentry user identifier** — if any error reports were tied to your
  account-type identifier, they are anonymized at deletion time.

## What is NOT deleted

- **De-identified research export rows** — if you opted into a research
  study, anonymized rows already exported under that study's IRB
  protocol remain in the research export (per the consent you signed
  for that study). The study consent describes the research-side
  withdrawal process separately.
- **Anonymized aggregate metrics** — counts and aggregate analytics
  that contain no identifier to you.
- **Legal-retention minimums** — anything we are required by law to
  retain (e.g., the fact of an account-deletion request itself), kept
  for the minimum statutory period.

## In-app deletion (Phase 1+)

When patient features ship in Phase 1, an in-app "Delete my account"
option will be available under **Settings → Account → Delete my
account**. It will trigger the same backend deletion as the email
request above, but executes immediately rather than requiring email
correspondence.

In Phase 0, the email-based process is the only path. We commit to
processing requests within the 30-day window above regardless.

## If you have questions before requesting

Email the same address: `jason.castellanos@gmail.com`. We will answer
questions about what would be deleted, how the process works, or what
to expect before you commit to deletion.

---

## Operational reference (internal)

For the IPMN Compass team — this section describes the implementation
of the deletion process and is included so the public-facing process
above stays honest.

Phase 0 (current):

1. Operator receives deletion-request email.
2. Acknowledge within 5 business days.
3. Within 30 days: open Firebase Console → Authentication → find user
   by email → delete user. Firestore Cloud Function trigger (Phase 1
   deliverable) will cascade-delete `users/{uid}` and all
   sub-collections. Until that Cloud Function lands, manual deletion of
   the Firestore user document is required at the same time.
4. Email the requester confirming completion.

Phase 1+ (in-app):

1. User taps "Delete my account" in app settings.
2. Confirmation modal.
3. Client calls `deleteAccountData` Cloud Function (queued — gated on
   first patient-data feature shipping per the privacy-policy.md "Phase
   1" data-collection set).
4. Cloud Function: cascade-delete Firestore docs → delete Firebase Auth
   record → return success.
5. Client clears local SecureStore + AsyncStorage entries scoped to the
   deleted uid (`services/legalAcceptance.ts` already has the per-uid
   cleanup pattern).
