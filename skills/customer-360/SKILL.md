---
name: customer-360
description: Give the full WindowEdge history of one homeowner (contact) - appointments, calls, texts, web chats, projects, quotes, invoices, notes and survey replies - with where the relationship stands and the next best action. Use when the user asks about a specific customer, homeowner, family or contact by name, phone or email.
argument-hint: "<name, phone or email>"
---

# Customer 360

Input: the homeowner's name, phone number or email.

1. Call `search_contacts`. If more than one contact matches, list them (name, city, last
   activity) and ask which one before you continue. If none match, say so and stop.
2. Call `get_contact` with the customerId for the profile and open tasks.
3. Call `get_contact_timeline` with the customerId for the merged history, quotes, invoices and
   survey replies.
4. If there are SMS conversations, call `get_sms_conversation` on the most recent one.
5. If there are voice calls, call `get_voice_call` on the most recent one.

## Write the answer

1. A timeline, oldest first: appointments, calls, texts, projects and notes, each with its date
   in the business timezone.
2. One paragraph on where the relationship stands now.
3. One recommended next action, with who should do it.

Quote the homeowner's own words where they matter. Do not repeat their phone number, email or
address unless the user asked for them.
