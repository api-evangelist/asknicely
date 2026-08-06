---
name: Survey an AskNicely customer
description: Add or update a single contact in AskNicely and send them an NPS/CSAT/5-Star survey, correctly handling contact rules and suppression.
api: openapi/asknicely-openapi.yml
operations: [triggerSurvey, getContact]
---

# Survey a customer with AskNicely

Use this when one customer has just had a service interaction and you want AskNicely to survey them.

## Before you start

- **Base URL** is per tenant: `https://{domain}.asknice.ly/api/v1`. Substitute the account's own subdomain.
- **Auth** is the account API key in the `X-apikey` **header**. Do not put it in the query string, even
  though some AskNicely help articles show that form — a key in a URL leaks into logs and referrers.
- **There is no sandbox.** Every call runs against the live account and can email a real customer.

## Steps

1. **Send the trigger.** Call `triggerSurvey` — `POST /contact/trigger`, form-encoded:
   - `email` (required)
   - `name`, or `firstname` + `lastname`
   - `segment` for grouping
   - any custom data fields, snake_case with a `_c` suffix (`branch_location_c=Portland`)
   - `delayminutes` to hold the send back (ignored for SMS, which always sends immediately)
   - `addcontact=true` if you want to import only and let the daily scheduler send

2. **Read `result[].survey_sent`, not the HTTP status.** This is the single most common mistake with this
   API. AskNicely returns **HTTP 200 with `success: true`** for two conditions where no survey was sent:
   - the contact was already surveyed inside the Global Contact Rule window
   - the contact is deactivated or has unsubscribed

   Both come back with `survey_sent: false` and an explanatory `msg`. Treat those as a **suppressed**
   outcome, not a success and not an error.

3. **Record the returned `id`.** That is the AskNicely contact id, and it is the same value returned as
   `contact_id` on survey responses and as `id` in the unsubscribe list. Store it to correlate later.

4. **Verify if it matters.** Call `getContact` — `GET /contact/get/{search}/{key}` with the URL-encoded
   email and `email` as the key — to confirm the record and read `active`, `lastemailed` and
   `unsubscribetime`.

## Rules

- **Never send `triggeremail=true` in production.** It overrides *all* contact rules and forces a send on
  every call. It exists for development only. Setting it to `false` does not disable sending — remove the
  parameter entirely.
- **`thendeactivate=true` only works together with `triggeremail=true`.** To deactivate regardless of
  whether a survey went out, use `removeContact` instead.
- **This endpoint has its own lower rate limit**: 100 requests per 10s and 500 per 60s, against 200/1000
  for the rest of the API. If you are surveying more than a handful of people, use the
  `survey-customers-in-bulk` skill instead — that is AskNicely's own guidance.
- **No idempotency key exists.** A retried call can send a second survey. On a timeout, verify with
  `getContact` before retrying.
- Multi-value custom fields are not supported here. Use `bulkAddContacts`.

## Errors

- `401` — key missing or not resolvable. `{"success": false, "msg": "We could not find the api key, or it was not set []"}`
- `429` — back off for `Retry-After`; check `RateLimit-Req10s-Remaining` / `RateLimit-Req60s-Remaining`.

Full catalog: `errors/asknicely-problem-types.yml`. Conventions: `conventions/asknicely-conventions.yml`.
