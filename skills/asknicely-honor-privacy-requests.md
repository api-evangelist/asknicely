---
name: Honour an AskNicely privacy or unsubscribe request
description: Process a GDPR erasure request, deactivate a contact, or reconcile the unsubscribe list — distinguishing the three very different operations and gating the irreversible one.
api: openapi/asknicely-openapi.yml
operations: [privacyRemoveContacts, removeContact, getUnsubscribedContacts, getContact, deactivateAllContacts]
---

# Honour a privacy or unsubscribe request

AskNicely has three distinct "stop contacting this person" operations with very different consequences.
Pick deliberately.

| Operation | Effect | Reversible |
|---|---|---|
| `removeContact` | Sets the contact **inactive**. Data retained. | Yes — re-adding reactivates |
| `privacyRemoveContacts` | **Erases personal data and blocklists** the person. | **No** |
| unsubscribe | Contact-initiated, via the email footer link. | Not by the API |

## GDPR erasure — irreversible

Call `privacyRemoveContacts` — `POST /privacy/remove`, either as JSON:

```json
{"skipnotify": 1, "contacts": ["john@example.com", "jane@example.com"]}
```

or form-encoded with a single `email` parameter. `skipnotify: 1` suppresses the notification to the
account's primary user (default `0` notifies). Success is `{"success": true, "msg": "contact removed"}`.

**Gate this behind explicit human approval every time.** It removes all personal data *and* adds the
address to the blocklist, so that person can never be added to AskNicely or surveyed again — including by
a later legitimate import. There is no undo and no idempotency key. Confirm the exact address list with a
human before sending, and log what you sent.

## Soft removal

Call `removeContact` — `GET /contact/remove/{search}/{key}` with the URL-encoded value and the key
(`email` by default, or `id`). This deactivates only; the record and its history stay. Re-adding the
contact reactivates them. Use this when someone asks to stop receiving surveys but has not asked for
erasure.

## Reconciling unsubscribes

Call `getUnsubscribedContacts` — `GET /contacts/unsubscribed`, optionally paged with `pagenumber`
(must be > 0, required if paging) and `pagesize` (defaults to 1000). Each row gives `id`, `email` and
`unsubscribetime`.

The returned `id` is **the same contact id** as `contact_id` on responses and the `id` returned by
`triggerSurvey`, so join on it directly.

**Re-importing an unsubscribed contact does not re-subscribe them** — AskNicely keeps the suppression. Do
not treat a successful bulk import as evidence that a previously unsubscribed person will be surveyed.

## Account-wide deactivation

`deactivateAllContacts` — `POST /contacts/deactivateall` — deactivates **every** contact in the account.
It returns HTTP **307** repeatedly until the job completes, so the client must follow redirects
(`curl --location --max-redirs 50`). This is not a privacy operation; it is a bulk stop-sending switch.
Require explicit human approval and never invoke it as a step in an automated flow.

## Rules

- Verify before and after with `getContact` — check `active` and `unsubscribetime`.
- Erasure and account-wide deactivation must never run unsupervised from an agent.
- None of these operations are idempotent in the API sense, but erasure is naturally terminal — a repeat
  call on an already-erased address is harmless.

Errors: `errors/asknicely-problem-types.yml`. Conventions: `conventions/asknicely-conventions.yml`.
