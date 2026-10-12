---
name: executive-brief
description: Produce a WindowEdge operating brief for an owner (daily or weekly), a sales manager, a marketing lead, or a customer-risk review, with sections, evidence and three recommended actions. Use when the user asks for a weekly recap, owner brief, sales manager brief, marketing brief or executive summary of the business.
argument-hint: "[daily_owner | weekly_owner | sales_manager | marketing | customer_risk]"
---

# Executive brief

Pick the brief type from the user's request:

| The user asks for | type |
|---|---|
| Today, for the owner | `daily_owner` |
| The last 7 days, a weekly recap | `weekly_owner` |
| Reps and close rates | `sales_manager` |
| Channels and the funnel | `marketing` |
| Customers at risk (last 30 days) | `customer_risk` |

If the request does not fit one type, ask which one. For a morning stand-up with names and
today's calendar, use the morning-briefing skill instead.

1. Call `get_tenant_profile` for the business name and timezone.
2. Call `get_executive_brief` with the type.
3. Present the brief section by section, then the three recommended actions.
4. If the brief's data-quality status is not ok, say which parts are missing.

Use only the figures the brief returned.
