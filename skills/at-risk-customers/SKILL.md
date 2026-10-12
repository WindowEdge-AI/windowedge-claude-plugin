---
name: at-risk-customers
description: Find WindowEdge homeowners at risk - unhappy customers, quotes that went quiet, unread or flagged conversations, unresulted appointments, overdue invoices and negative survey replies - with the signal and a suggested owner for each. Use when the user asks who is unhappy, who is at risk, who needs a call back, or which deals are going cold.
argument-hint: "[days]"
---

# At-risk customers

Input: a look-back window in days (default 30).

1. Call `get_tenant_profile` for the timezone.
2. Call `run_diagnostic` with `type` = `customer_risk`.
3. Call `get_attention_queue` for flagged threads, unresulted appointments, overdue invoices and
   negative survey replies.
4. Call `query_analytics` with `metric` = `angry_customers` and `periodStart` and `periodEnd`
   (YYYY-MM-DD) for the window.
5. Call `list_sms_conversations` twice, with `filter` = `unread` and with `filter` = `flagged`,
   both with `sinceDays` = N.
6. Call `list_voice_calls` with `filter` = `flagged` and `sinceDays` = N.
7. Call `get_pipeline_status` with `stalledAfterDays` = 21 for stalled projects and the oldest
   open quotes. Count a quote as at risk only when the result shows its age or days in stage. A
   quote status alone is not a risk signal.

## Write the answer

Group the people by urgency: act today, act this week, watch. For each person, write one line:

- name
- the signal, in the homeowner's own words when available
- how long it has been
- the suggested owner

Remove duplicates: one person appears once, with all of their signals. Do not pad the list. If
it is short, say so.
