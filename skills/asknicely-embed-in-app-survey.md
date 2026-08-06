---
name: Embed an AskNicely survey in your own app
description: Negotiate a one-time AskNicely survey slug with an HMAC-signed contact identity so a survey renders inside your website or mobile application instead of arriving by email or SMS.
api: openapi/asknicely-inapp-openapi.yml
operations: [requestSurveySlug]
---

# Embed an AskNicely survey in your own app

Use this when the survey should appear inside your product surface rather than in the customer's inbox.
The flow is three steps: sign the identity on your server, exchange it for a slug, render the slug.

## 1. Sign the email on your server

Compute an HMAC-SHA256 of the contact's email address, keyed with your AskNicely account secret key, and
hex-encode it. AskNicely publishes worked examples in PHP, Ruby, Python, Java and C#, and hosts a live
hash-verification widget on the docs page so you can confirm your implementation before you integrate.

```
email_hash = hex(hmac_sha256(key = <account secret key>, message = <contact email>))
```

**The secret key must never reach the browser or the mobile binary.** Compute the hash server-side and
hand only the finished hash to the client. The hash is what authenticates this request — there is no
`X-apikey` on this endpoint.

## 2. Exchange it for a slug

Call `requestSurveySlug` — `POST /service/inapp.php?id={uuid}` on the tenant host. Note this endpoint sits
**outside** `/api/v1`.

- `id` (query, required): a **single-use, dash-free, 16-character UUID**. AskNicely uses it to prevent
  duplicate submissions — generate a fresh one per request and never replay it.
- Body (JSON), all required unless noted:
  - `domain_key` — the domain you signed up to AskNicely with
  - `template_name` — the survey template to use
  - `name` — the person being surveyed
  - `email` — their email address
  - `email_hash` — the hash from step 1
  - `created` — unix timestamp (seconds) of when this customer joined your service
  - `force` (optional) — see the warning below
  - any number of additional custom properties

## 3. Render the survey

Use the returned survey slug to display the survey to the user. The slug identifies who is answering and
binds their response back to the contact record, so responses arrive alongside email and SMS responses in
`getResponses` and fire the same webhook.

For native apps, AskNicely publishes Android and iOS code samples at
`https://github.com/asknicely/asknicely-mobile-inapp-samples`. There is no packaged SDK — the samples are
source you copy, not a dependency you install.

## Rules

- **Never ship `force: true` to production.** It ignores contact rules *and* prevents the survey from
  triggering workflows, so responses collected with it will not drive your automations. It exists for
  development only.
- Reuse of the `id` UUID is treated as duplicate spam. One UUID, one request.
- `created` is a unix timestamp in **seconds**, sent as a string.
- The account secret key is a different credential from the `X-apikey` API key. Do not interchange them.

Conventions: `conventions/asknicely-conventions.yml`. Components: `components/asknicely-components.yml`.
