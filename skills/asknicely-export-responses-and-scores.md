---
name: Export AskNicely responses and read scores
description: Page survey responses out of AskNicely into a warehouse or BI tool and read NPS, sent and historical statistics, handling the redirect and positional-filter behaviour correctly.
api: openapi/asknicely-openapi.yml
operations: [getResponses, getNps, getSentStats, getHistoricalStats]
---

# Export responses and read scores

Use this to pull feedback into a warehouse, Tableau/PowerBI, or to answer a score question.

## Export responses

Call `getResponses` — `GET /responses/{sortDirection}/{pagesize}/{pagenumber}/{sinceTime}/{format}`.
The published template also accepts three further optional trailing segments:
`/{filter}/{sort_by}/{end_time}`.

- `sortDirection`: `asc` (default) or `desc`
- `pagesize`: **maximum and default are both 50,000**. Requests above 50,000 error.
- `pagenumber`: first page is `1`
- `sinceTime`: unix timestamp lower bound — pass `0` for everything
- `format`: `json` or `csv`
- `filter`: `answered` (responses only), `raw` (**all surveys sent, including unanswered**), `published` (testimonials only)
- `sort_by`: `sent` or `responded`
- `end_time`: unix timestamp upper bound
- `?includestatustime=yes` adds `case_closed_time` to each row

**Your client MUST follow redirects.** Above the redirect threshold (999 responses as of 3 December 2022,
and AskNicely warns the number may change) the response is a redirect to a temporary file on S3. A client
with redirects disabled gets nothing and no error. Use `curl --location` or the equivalent.

### Paging loop

1. Request page 1 with `pagesize=50000`.
2. Read `total` and `totalpages` off the envelope.
3. Increment `pagenumber` until you have `totalpages` pages.
4. On the next run, set `sinceTime` to the highest `responded` timestamp you already hold.

Each row carries `response_id`, `contact_id` (use this, not the deprecated `person_id`), `answer`,
`comment`, `question_type` (`nps` / `csat` / `fivestar`), the sent/opened/responded unix timestamps, and
every custom data field on the contact.

## Read scores

- `getNps` — `GET /getnps/{days}` returns `{"NPS": "64.5"}` for a rolling window (default 30 days).
- `getSentStats` — `GET /sentstats/{days}` returns sent, delivered, opened, responded, promoters,
  passives, detractors and `responserate`. The template also accepts `/{field}/{value}` for one filter,
  including `/question_type/csat`.
- `getHistoricalStats` — `GET /stats` with `year` / `month` / `day`, or `start_time` / `end_time` unix
  bounds, plus optional `segment`. Returns day-by-day rows including `nps`, `csat` and `fivestar`.

## Filtering — read this before you build a query

`getResponses`, `getNps` and `getSentStats` accept repeatable `filters[]` and `values[]` query
parameters. **They are paired by POSITION.**

```
?filters[]=country_c&filters[]=city_c&values[]=Canada&values[]=Abbotsford
```

The counts must match and the order must match. A mismatch returns **an empty result set, not an error** —
so a wrong query looks exactly like "no data". Always assert `len(filters) == len(values)` before sending,
and treat an empty result on a filtered query as suspect until you have verified it unfiltered.

## Rules

- All numbers and timestamps come back as **strings**. Cast on your side.
- Timestamps are unix epoch **seconds**.
- Filtering is limited to response timestamps plus custom-field equality — there is no rich query language.
- Rate limits are 200 per 10s and 1000 per 60s. Honour `Retry-After` on a 429.
- These are read operations and are safe to retry.

Conventions: `conventions/asknicely-conventions.yml`. Data model: `data-model/asknicely-data-model.yml`.
