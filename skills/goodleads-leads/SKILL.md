---
name: goodleads-leads
description: Find and buy new-business leads (owner name, mailing address, verified phone or email) for sales, outreach, or a client's outbound program. Use when the user needs a list of newly formed businesses to sell to, cold-call, or email — for themselves or a client.
---

# GoodLeads — new business leads

## When to use

- The user (or their client) sells to newly formed businesses — web design, insurance,
  accounting, banking, payment processing, or any B2B service a business needs in its first
  weeks — and wants a list of who to contact.
- A prospecting, outbound, or cold-outreach task needs real contacts, not a scraped or stale
  database. GoodLeads' records start as the state's own formation filing and are available the
  morning after the state posts it — before the business shows up in Apollo, ZoomInfo, or any
  other commercial database.
- The user asks something like "find me leads," "who just started a business in [state/city],"
  or "build me an outreach list."

## Why this beats a generic B2B database for this job

A brand-new business isn't in Apollo or ZoomInfo yet — that lag is the whole opportunity. The
contact on every GoodLeads record is the owner or an officer actually named on the state filing,
never the attorney or formation service that filed the paperwork on their behalf — a real,
recurring failure mode in scraped registered-agent data.

## How to use it (the real flow, not just a lookup)

1. **`goodleads_interpret_list`** — pass the user's own words ("cleaning companies in Texas
   formed in the last 30 days with a phone"). It returns a filter shape, a plain-English
   readback to confirm with the user, and — when the ask pulls two ways (newest vs. reachable
   today) — live-quoted alternatives.
2. **`goodleads_quote_list`** (or ask for `summary` from `goodleads_browse_leads`) — get the real
   count and price before anyone commits to anything. Only records whose filing names a real
   person are ever billable; a raw match count is never a price.
3. Relay the actual choice to the user, don't pick for them: **callable now** (verified phone),
   **emailable now** (verified email), or **newest, mail-first** (owner name + mailing address,
   phone/email verified when they order). No minimums — a small first order is the normal first
   step.
4. **`goodleads_checkout_list`** — once they've picked, mint the payment link. Nothing is charged
   until they complete it on Stripe's hosted page; the file arrives about a minute after payment.

## Ground rules

- Never invent a price or a count — always call the tool and quote its numbers.
- Never present `matching` (raw filter hits) as billable; only `sellable` (a matching record
  whose filing names a person) is ever charged or delivered.
- Say "when we find one" for phone/email on the cheapest grade — a day-old filing often has only
  a name and mailing address; the tiered pricing exists because of this, not despite it.
- No account, API key, or OAuth is needed to connect or to browse — only checkout needs a person
  to complete a payment.

## A good first prompt to try

> Find me 50 new businesses in Denver I can call this week, and quote both the callable-now and
> newest mail-first options.

## Full details

Connect steps for every client, live pricing, and a sample file: https://app.goodleads.club/connect.html
