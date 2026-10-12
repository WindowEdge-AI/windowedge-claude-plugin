---
name: marketing-review
description: Review WindowEdge marketing performance - spend, leads, appointments and sales by channel and campaign, cost per lead and per sale, the funnel drop-off, unworked leads, and what to cut or grow. Use when the user asks about ads, marketing channels, lead sources, cost per lead, wasted spend or campaign ROI.
argument-hint: "[days]"
---

# Marketing review

Input: a look-back window in days (default 30).

1. Call `get_tenant_profile` for the timezone.
2. Call `get_marketing_performance` with `period` = "last N days" for spend, cost per lead, per
   appointment and per sale, by campaign and channel.
3. Call `query_analytics` once for each of these values of `metric`, with `periodStart` and
   `periodEnd` (YYYY-MM-DD) for the same window: `marketing_channel_performance`,
   `marketing_funnel`, `marketing_poor_performance` and `marketing_unbooked_by_source`.
4. Call `run_diagnostic` with `type` = `campaign_waste` and `days` = N.
5. Call `list_leads` with `status` = `new` and `sinceDays` = N for leads that nobody has worked.

## Write the answer

1. A channel table ranked by sales: spend, leads, appointments, sales, cost per lead, cost per
   sale.
2. The funnel, with the biggest drop-off named.
3. Unbooked leads by source. Call out opt-outs and landlines.
4. Three concrete actions: what to cut, what to grow, what to fix.

Use only the numbers the tools returned. If spend is missing for a channel, say so. Do not
calculate a cost per lead from a spend figure you do not have.
