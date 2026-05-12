# IPMN Compass — Privacy Policy

**Effective Date:** 2026-05-12 (Phase 0 — pre-MVP)

> IPMN Compass is in pre-MVP development. The app does not yet collect or
> process patient health information. This policy describes the
> boundaries that will apply when patient features ship in Phase 1 and
> after.

This is the publicly-hosted equivalent of the in-app Privacy Policy
screen (`app/privacy-policy.tsx`). The two must stay in sync; a future
verifier (`scripts/verifyPrivacyConsistency.ts`, queued) will enforce
the alignment.

---

## Overview

IPMN Compass ("the App") is a clinic-visit companion for clinicians and
patients navigating intraductal papillary mucinous neoplasm (IPMN)
management decisions. It is not a calculator and not a medical device.
This Privacy Policy describes what information the App collects, how it
is stored, and your rights regarding your data.

## What we collect today

In Phase 0 (current), the App collects only the minimum needed to
authenticate testers:

- A Firebase Authentication account (email + password or OAuth via
  Google or Apple).
- Your acceptance record for this Privacy Policy + the Terms of Service
  + the Medical Disclaimer.
- For the clinician-track surfaces, an attestation that you are a
  licensed clinician (stored on your device only, never transmitted).

No patient health information is collected in Phase 0.

## What we will collect in Phase 1

As patient-facing features ship, the App will collect only the
information needed to render the multi-guideline view and patient
education content:

- Cyst features you enter (size, location, main-duct diameter,
  mural-nodule status) — written to your account in Firestore so the
  clinician view + your follow-up visits can reference them.
- Symptoms you report (jaundice, pancreatitis, weight loss, abdominal
  pain) — same storage.
- Surveillance imaging dates you confirm — date offsets stored (not
  absolute dates) when used for research export
  (`INV-IPMN-PHI-006`).
- Your reading-level preference (plain / standard / clinical), so
  content renders appropriately.

**We do NOT collect:**

- Your full date of birth (`INV-IPMN-PHI-003`).
- Your radiology report text in any persistent store
  (`INV-IPMN-PHI-001`).
- Accession numbers in plaintext (`INV-IPMN-PHI-005`).
- Your institution name in free text (`INV-IPMN-PHI-007`).

## How your data is stored

Account information and patient-entered features are stored using
Google Firebase (Firestore + Authentication) under a HIPAA Business
Associate Agreement with Google Cloud. Data is encrypted in transit
(TLS) and at rest (Google Cloud default encryption). Each user's data
is isolated to their own authenticated account by Firestore security
rules; other users cannot access it. Server-side rules reject any
write containing a name, full date of birth, plaintext accession
number, or free-text institution name.

## AI-powered features

**Our boundary: we do not automatically send personal health
information to AI services.**

When AI features ship in Phase 2, they will route through a vendor
(Anthropic Claude) that has signed a HIPAA Business Associate Agreement
covering this project, and you will be asked for explicit consent
specifically authorizing AI processing of the relevant content. No AI
features are active in Phase 0.

## Push notifications

The App will send push notifications through Apple Push Notification
Service and Google's Firebase Cloud Messaging in Phase 1+. Lock-screen
payloads are generic and never include your name or any clinical
information ("IPMN Compass — Your surveillance schedule has been
updated"). The actual content is fetched only after you open the App
and authenticate, by reading a record gated to your account.

## Research participation

Research features — including any IPMN registry, decision-model
calibration study, or affiliated cooperative-group research — are
strictly opt-in and presented separately from standard use of the App.
Research participation requires a separate informed consent. Research
exports are de-identified before transmission: dates are converted to
per-subject offsets from an anchor date (`INV-IPMN-PHI-006`),
institution names are coded (`INV-IPMN-PHI-007`), and small-cell rows
pass a k-anonymity check before emission (`INV-IPMN-PHI-008`). You can
use the App without participating in any research.

## Crash reports and analytics

The App uses Sentry for crash and error reports. Reports do not contain
your name, your stored clinical features, or any personally
identifiable information. The App will use Firebase Analytics in Phase
1+ for product-usage events (which screens you opened, how long a flow
took); events carry only categorical labels and durations, never
clinical readings, free-text notes, or identifiers.

## Data sharing

We do not share, sell, rent, or trade your personal information with
third parties. External data transmission is limited to:

- Authentication and encrypted data storage (Google Firebase, under
  HIPAA BAA).
- Anonymized crash reports (Sentry).
- Categorical product-usage events (Firebase Analytics — Phase 1+).
- AI feature requests (Anthropic Claude — Phase 2+, opt-in, BAA-gated,
  content-specific consent).
- De-identified research data, only if you opt into a specific
  research study.

## Your rights

You have the right to:

- **Delete all App data at any time** — see `docs/policies/data-deletion.md`
  for the current process. The Phase 1 in-app deletion callable lands
  with the first patient-data feature; before then, email the contact
  address below to request deletion.
- **Decline optional features.**
- **Withdraw from any research study you have joined.**
- **Request information about your stored data** by emailing the
  address below.

## Children's privacy

The App is not intended for use by individuals under the age of 18. We
do not knowingly collect information from children.

## Changes to this policy

We will update this policy as features ship. Changes will be posted
within the App and the effective date will be updated. For substantive
changes (new data collection, new vendor) the App will prompt for
re-acknowledgment.

## Contact us

IPMN Compass project — `jason.castellanos@gmail.com` (interim;
institutional contact lands with the NYU-as-research-sponsor decision
per `docs/decisions/2026-05-12-nyu-as-research-sponsor.md`).
