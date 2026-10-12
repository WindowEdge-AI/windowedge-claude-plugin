---
name: rep-scorecard
description: Build a performance scorecard for one WindowEdge sales rep - close rate against the team, appointments booked, run and sold, cancellations, sold value, utilization, upcoming calendar and open pipeline, with coaching notes. Use when the user asks how a rep is doing, for a rep review, or to compare a rep with the team.
argument-hint: "<rep name> [days]"
---

# Rep scorecard

Inputs: the rep's name, and a look-back window in days (default 30).

1. Call `get_tenant_profile` for the timezone.
2. Call `list_team_members` and resolve the rep's name to one person and their repId. If more
   than one person matches, list them and ask which one. If nobody matches, say so and stop.
3. Call `get_rep_scoreboard` with `period` = "last N days". Read this rep's row against the
   team.
4. Call `query_analytics` with `metric` = `rep_utilization`, `periodStart` and `periodEnd` for
   the same window (YYYY-MM-DD), and `groupBy` = `["sales_rep"]`. Read this rep's row.
5. Call `get_schedule` with `rep` = the rep's name, `start` = today and `end` = 13 days from
   today (YYYY-MM-DD). Use its per-rep count for upcoming appointments.
6. Call `list_projects` with `status` = `open`, the rep's `salesRepId` and `limit` = 100. If it
   returns 100 projects, the list is cut off: report the value as "at least" that amount, and say
   the full total is not available.

## Write the answer

Report, for the window:

- close rate, next to the team rate
- appointments booked, run and sold
- cancellations and no-shows
- sold value, and its change from the prior period
- utilization
- upcoming appointments (next 14 days)
- open pipeline value

End with two coaching observations. Each one must point to a number above. Do not invent figures.
If a tool did not return a value, write "not available".
