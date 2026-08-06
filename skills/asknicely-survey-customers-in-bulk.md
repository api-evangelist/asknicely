---
name: Bulk load AskNicely contacts and survey them
description: Import a batch of contacts into AskNicely from JSON or a CSV importer, optionally triggering surveys under the Global Contact Rules, and verify an asynchronous job that returns no per-contact result.
api: openapi/asknicely-openapi.yml
operations: [bulkAddContacts, uploadContactsCsv, getContact]
---

# Bulk load and survey contacts

Use this whenever you have more than a handful of contacts. AskNicely explicitly recommends batching here
rather than looping `triggerSurvey`, which carries a lower rate limit.

## Path A — JSON batch

Call `bulkAddContacts` — `POST /contacts/add` with `Content-Type: application/json`:

```json
{
  "contacts": [
    {"name": "Jane Doe", "email": "jane@example.com", "region_c": "West", "account_owner_c": "Sarah"},
    {"name": "John Smith", "email": "john@example.com", "region_c": "East", "account_owner_c": "Tom"}
  ],
  "obeyrules": true
}
```

- `contacts` must be an **array**; `email` is the only required field per contact.
- Custom data fields use the `_c` suffix and **may carry an array of values** here (unlike `triggerSurvey`).
- `"obeyrules": true` triggers surveys for contacts eligible under the Global Contact Rules.
  **`false` is not a valid value** — omit the property entirely to import without sending.
- Payload cap is **6MB**. Split larger loads.

## Path B — CSV importer

Call `uploadContactsCsv` — `POST /curlupload/{importerId}` as `multipart/form-data` with a `file` part.
The importer must already exist (created in the CSV Importer App); its id is shown on the last step of
setup. The importer's own configuration decides column mapping and whether surveys are sent. Same 6MB cap.

## Steps

1. Chunk the batch so each request stays under 6MB.
2. POST the batch.
3. Expect **HTTP 201** with `{"success": true}` and nothing else. This endpoint has been **asynchronous
   since 1 July 2021** — it validates the payload, accepts it, and processes afterwards. There is **no
   per-contact result, no job id, no status endpoint and no completion callback.**
4. Verify by sampling: call `getContact` — `GET /contact/get/{email}/email` — on a few addresses from the
   batch and check `importedattime` and `lastemailed`.

## Rules

- **Do not blind-retry a timed-out batch.** Because the job is fire-and-forget with no idempotency key, a
  retry can double-process it. Verify with `getContact` first.
- Adding an unsubscribed contact **does not re-subscribe them**. They stay suppressed.
- Contacts on the Blocklist — including anyone erased through `privacyRemoveContacts` — are silently not
  imported.
- The exception in the Global Contact Rules for *newly added* contacts is **not** applied by this endpoint;
  only the first rule is.

## Errors

All are HTTP 400 unless noted, in the `{"success": false, "msg": "..."}` envelope:

- `no data found` — no body, or no `contacts` property.
- `Errors found in json - {error}` — malformed or badly encoded JSON. Validate and UTF-8 encode first.
- `if set, contacts must be an array` — `contacts` was an object or a string.
- `500 Internal Server Error` — also returned when the body exceeds 6MB.
- `401 Could not find user with API key` — on the CSV path, an unresolvable key.

Full catalog: `errors/asknicely-problem-types.yml`.
