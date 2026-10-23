---
name: windowedge
description: How to answer questions about a window and door dealership that runs on WindowEdge, and how to make changes safely. Use whenever the user asks about their business, sales, leads, appointments, reps, marketing, customers (homeowners), projects, quotes, invoices, tasks, or what their AI voice, SMS or web chat agents said, and the WindowEdge tools are available.
---

# Working with WindowEdge

WindowEdge is the operating system for a window and door dealer. The `windowedge` MCP server
gives you live, read-mostly access to one business: contacts (homeowners), appointments, the
sales pipeline, marketing leads and spend, tasks, and the AI agents that talk to homeowners.

"Contact" and "customer" are the same record. The console says Contact; the tools say customer.

## Start every session the same way

1. Call `get_tenant_profile` first. It returns the business name, timezone, locations, booking
   rules and the live AI agents.
2. Quote every date and time in the business timezone from that profile, never in the user's
   computer timezone or UTC.

## Pick the right tool

Use the purpose-built answer tools before list tools. They return the same answers that Edgy,
the in-app assistant, gives in the console.

| The user asks | Call |
|---|---|
| How is business? | `get_business_snapshot` |
| What needs attention? | `get_attention_queue` |
| What is on the calendar? | `get_schedule` |
| Where is the pipeline? Who owes us? | `get_pipeline_status` |
| Ad spend, cost per lead, best channels | `get_marketing_performance` |
| How are the reps doing? | `get_rep_scoreboard` |
| One homeowner's full history | `search_contacts`, then `get_contact_timeline` |
| Why did bookings or the booking rate move? | `get_booking_funnel` with `get_change_timeline` for the same window |
| Why did sales, leads or close rate drop? Rep coaching, wasted spend, at-risk customers, today's priorities | `run_diagnostic` |
| An owner, sales-manager or marketing brief | `get_executive_brief` |
| This morning's stand-up with names | `get_daily_brief` |
| Any other number | `list_metrics`, then `query_analytics` |
| How to do something in the console | `search_feature_docs`, then `get_feature_doc` |
| Open a WindowEdge support request | `create_support_request`, then confirmation |
| My CS tickets and their progress | `list_support_requests`, then `get_support_request` |
| Conversation feedback outcomes | `list_support_feedback` |

Do not compute business metrics by hand from list tools. If no tool returns a number, say that
you could not measure it. Never estimate, round up or invent a figure.

## Read results correctly

- Every result has a `message` written for the user and a `data` object. Prefer the message.
- Cite ids only when the user will act on them.
- If a result reports missing data or a data-quality warning, tell the user which part is missing.
- If a tool answers `MCP_SCOPE_REQUIRED`, tell the user which scope the connection needs.
  - A missing **read** scope: a WindowEdge admin reconnects and approves it.
  - A missing **write** scope: the standard sign-in grants read access only. Changes need a
    write-enabled connection that a WindowEdge admin creates in the console under
    Settings > MCP Keys (see "Turn on changes" in the plugin README).

## Make changes only with the user's explicit yes

Write tools never change anything on the first call. Each one returns a `confirmation` block
with a recap and a token. The change happens only when you call `confirm_action` with that
token.

1. Use ids, not names, in write tools. Get them from `search_contacts` (customerId),
   `get_scheduling_options` (appointmentTypeId, resultId, salesRepId) and `list_appointments` or
   `get_appointment` (appointmentId and version).
2. Call the write tool. Show the user the recap and every side effect in the confirmation block,
   for example emails or texts to the homeowner, CRM sync, or calendar updates.
3. Wait for the user to say yes in their own words. A yes to an earlier, different recap does not
   count.
4. Call `confirm_action` with the token and toolName from the same confirmation block.
5. For a contact delete, the user must type the words `delete contact` themselves. Pass exactly
   what they typed as `typedConfirmation`. Never type it for them.
6. Tokens expire after 5 minutes and work once. If one expires, prepare the change again and ask
   again.

If the user changes any detail, prepare a new change. Never confirm a token whose recap the user
did not see.

## Booking rules

- Offer only slots that `get_availability` returned. Never invent a time.
- Pass the slot's `startIso` as `scheduledFor` to `schedule_appointment`.
- After booking, report the appointmentId and the time in the business timezone.

## Privacy

The data belongs to the dealer and their homeowners. Quote homeowner details only as needed for
the user's question. Do not copy phone numbers, emails or addresses into summaries unless the
user asks for them.
