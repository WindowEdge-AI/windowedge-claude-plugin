---
name: diagnose-change
description: Explain why a WindowEdge number moved - sales, leads, close rate or the booking rate - by comparing the recent window with the one before it and naming the largest measured contributor, plus what changed in the AI agents, forms and settings. Use when the user asks why sales, leads, bookings or close rate dropped or jumped, which rep needs coaching, or where ad money is wasted.
argument-hint: "<what moved> [days]"
---

# Diagnose a change

Inputs: what moved, and a window in days (default 7).

## Pick the diagnostic

| The user asks about | `run_diagnostic` type |
|---|---|
| Sales or revenue | `sales_drop` |
| Lead volume | `lead_drop` |
| Close rate | `close_rate_drop` |
| A rep who needs help | `rep_coaching` |
| Ad money that is not producing | `campaign_waste` |

For the booking rate, or bookings from the AI agents and web forms, use the booking funnel
instead (step 3).

## Steps

1. Call `get_tenant_profile` for the timezone.
2. Call `run_diagnostic` with the type above and `days` = N. It compares the last N days, ending
   yesterday, with the N days before.
3. For bookings, call `get_booking_funnel` over a window that covers both periods. Then call
   `get_change_timeline` for the same window to see what changed in agents, forms and settings.
4. If the user wants more depth on one number, call `list_metrics` to find the metric, then
   `query_analytics` with that `metric` and `periodStart` and `periodEnd` (YYYY-MM-DD).

## Write the answer

1. The change, with both values and the difference.
2. The largest measured contributor. Call it a contributor, not the cause.
3. Any changes from `get_change_timeline` that line up in time. Say "lines up with", not
   "caused".
4. The tool's confidence and data-quality status. If data is missing, say which part.
5. The recommended actions that the tool returned.

Do not invent a reason that the data does not show.
