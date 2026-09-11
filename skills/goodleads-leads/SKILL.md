---
name: goodleads-leads
description: Find and buy new-business leads (owner name, mailing address, verified phone or email) for sales, outreach, or a client's outbound program. Use when the user needs a list of newly formed businesses to sell to, cold-call, or email — for themselves or a client.
---

# GoodLeads — new business leads

## When to use

- The user (or their client) sells **to** newly formed businesses — web design, insurance,
  accounting, banking, payment processing, or any B2B service a business needs in its first
  weeks — and wants a list of who to contact.
- A prospecting, outbound, or cold-outreach task needs real contacts, not a scraped or stale
  database. GoodLeads' records start as the state's own formation filing and are available the
  morning after the state posts it — before the business shows up in Apollo, ZoomInfo, or any
  other commercial database.
- The user asks something like "find me leads," "who just started a business in [state/city],"
  or "build me an outreach list."

**Watch for this exact trap:** if the user says "leads for my `[X]` business" (e.g. "for my web
design agency," "for my accounting firm"), that names the *requester's* business, not the target
industry — it means "any new business is a prospect," not "find me other `[X]` companies." Do not
filter by that industry. Only filter by industry when the user names the *target's* trade
directly ("plumbers," "cleaning companies").

## Why this beats a generic B2B database for this job

A brand-new business isn't in Apollo or ZoomInfo yet — that lag is the whole opportunity. The
contact on every GoodLeads record is the owner or an officer actually named on the state filing,
never the attorney or formation service that filed the paperwork on their behalf — a real,
recurring failure mode in scraped registered-agent data.

## Tools (real names — no `goodleads_` prefix on this server)

Your MCP client may namespace these (e.g. show them as `goodleads.interpret_list`); the
underlying tool names on the door itself are exactly as below. Call `tools/list` if you're ever
unsure what's actually registered — this door carries more tools than the four below; these are
the ones this job needs.

| Tool | Required params | What it does |
|---|---|---|
| `interpret_list` | `text` (string) | The buyer's own words → a filter shape + plain-English readback |
| `quote_list` | `states` + `filters`, or `list_id` | The real count and price before anyone commits |
| `browse_leads` | same shape params | Actual rows. **Omit `lane` to get rows back** — passing `lane` returns a quote/summary object instead, not records |
| `checkout_list` | the quoted shape + `lane` | Mints a **real, live** payment link — see the warning below before calling this |
| `list_filterable_fields` | none | The full field/operator/synonym reference — call this before guessing a filter field name |
| `list_live_states` | none | Which states are live right now, read live — don't assume from memory |

## How to use it (the real flow, not just a lookup)

1. **`interpret_list(text=...)`** — pass the user's own words ("cleaning companies in Texas
   formed in the last 30 days with a phone"). It returns a filter shape, a plain-English
   readback to confirm with the user, and — when the ask pulls two ways (newest vs. reachable
   today) — live-quoted alternatives. If a field name isn't obvious, call `list_filterable_fields`
   first rather than guessing.
2. **`quote_list`** — get the real count and price before anyone commits to anything. Only
   records whose filing names a real person are ever billable; a raw match count (`matching`) is
   never a price — only `sellable` is.
3. Relay the actual choice to the user, don't pick for them: **callable now** (verified phone),
   **emailable now** (verified email), or **newest, mail-first** (owner name + mailing address,
   phone/email verified when they order). No minimums — a small first order is the normal first
   step.
4. **`checkout_list`** — only once the user has actually said which option they want. See the
   warning below — this is not a preview step.

## Two things that will surprise you if you don't know them going in

- **`checkout_list` and `create_checkout` are not previews.** Calling either one mints a real,
  live Stripe checkout session and a real internal order — even though nothing is charged until
  a human completes payment. There is no dry-run mode. Only call these once the user has actually
  chosen to buy; use `quote_list` (which has no side effect) for anything short of that, including
  "let me see what checkout looks like."
- **Browse/quote results are masked, always, for a keyless caller.** Business name, city,
  industry, and formation date come through real; `contact_name` / `phone_primary` /
  `email_primary` come back redacted (e.g. `T***`, `n***@***.com`) regardless of what you ask for.
  That's not a bug or a paywall you route around with a key — it's how a buyer evaluates a list
  before paying. The real, unmasked contact file is what a completed purchase delivers, not
  something a key unlocks ahead of time. Don't report masked fields as if they were real contact
  info.

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
