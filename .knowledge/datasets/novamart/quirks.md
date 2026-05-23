# NovaMart — Data Quirks

Known gotchas in the NovaMart practice dataset. Read this alongside `schema.md`
before designing queries — these are the things that quietly produce wrong
answers.

## Coverage
- Data covers **2024** — one full year, 13 tables.
- `events` runs Jan 1 2024 through Jan 1 2025; every other table is within 2024.

## events
- `event_type` has **10 values**, not just the funnel steps: `page_view`,
  `product_view`, `add_to_cart`, `checkout_started`, `payment_attempted`,
  `purchase_complete`, plus `login`, `app_open`, `search`, `save_for_later`.
  Always filter to the event types you want — never `GROUP BY event_type` raw
  and assume it's a funnel.
- The purchase funnel is `page_view → product_view → add_to_cart →
  checkout_started → purchase_complete`. Measure each step as
  `COUNT(DISTINCT user_id)` — it is a per-step distinct-user funnel, not a
  strict per-user sequence.

## memberships
- **Multiple rows per user.** ~5,500 rows for ~3,800 users — a user who starts
  on a trial and converts has more than one row. Joining `memberships` to
  `orders` or `users` on `user_id` without de-duplicating inflates counts.
- `plan_type` is `plus_trial`, `plus_monthly`, `plus_annual`. Trials are the
  majority of the table.
- `status` is `active`, `cancelled`, `expired`, `converted`. Trials end as
  `expired` or `converted`; paid plans end as `cancelled`.
- `cancel_reason` is set **only** on `cancelled` rows (`price`, `not_using`,
  `competitor`, `other`) — it is null for expired trials and active members.

## orders
- `status` is `completed`, `cancelled`, `returned`. Use `status = 'completed'`
  for revenue and conversion — the others are not realized revenue.

## device
- Device appears in two places: `orders.device` (the device used for that
  order) and `users.device_primary` (the user's main device). They can
  disagree. Pick the one that matches your question — `orders.device` for
  order-level metrics.

## nps_responses
- Only ~2,400 of 8,000 rows have a `comment`; the rest are null.
- The comments are a **small recurring set of phrases** (~20 distinct
  strings), not unique free text — theming them is a lookup, not clustering.
- Comments mix praise and complaints. Filter to complaints before ranking
  issues.

## support_tickets
- Fully structured — **no free-text field**. Issues are pre-categorized in
  `category` (`delivery_issue`, `payment_issue`, `product_quality`,
  `account_issue`, `membership_issue`, `other`) with a `severity`.
