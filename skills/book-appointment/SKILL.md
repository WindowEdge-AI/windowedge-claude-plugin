---
name: book-appointment
description: Book a homeowner appointment in WindowEdge - find the contact, choose the appointment type and rep, offer real open slots, then schedule it only after the user confirms. Use when the user asks to book, schedule or set an appointment, consultation or measure for a homeowner.
argument-hint: "<customer> [when] [rep]"
---

# Book an appointment

Inputs: the homeowner (name, phone or email), an optional preferred day or time, and an optional
rep.

1. Call `get_tenant_profile` for the timezone and booking rules.
2. Call `search_contacts`.
   - More than one match: list them and ask which one.
   - No match: offer to create the contact with `create_customer`. That is a change, so follow the
     confirmation steps below before you continue.
3. Call `get_scheduling_options` for the appointment type id and, if the user named a rep, the
   salesRepId.
4. Call `get_availability` for the requested window with the `appointmentTypeId` from step 3
   (it sets the slot length). If the user named a rep, also pass that rep's `salesRepId`. Pass the
   homeowner's ZIP code as `zip` if you have it.
5. Offer up to three slots that `get_availability` returned, in the business timezone. Never
   offer a time it did not return.
6. After the user picks a slot, call `schedule_appointment` with:
   - the customerId
   - the same `appointmentTypeId`
   - the slot's `startIso` as `scheduledFor`
   - the slot's `durationMinutes` as `durationMinutes`
   - the slot's `salesRepId`, if it has one, as `assignedSalesRepId`. Do not assign a rep that the
     slot was not checked for.

If the user changes the type, the rep or the day, call `get_availability` again before you
prepare the booking.

## Confirm before anything happens

`schedule_appointment` does not book on the first call. It returns a confirmation block.

1. Show the user the recap and the side effects, for example a confirmation email or text to the
   homeowner, CRM sync and calendar updates.
2. Wait for an explicit yes from the user.
3. Call `confirm_action` with the token and toolName from that confirmation block.
4. Report the appointmentId and the time in the business timezone.

If the user changes the slot, the type or the rep, prepare a new booking and confirm again. If the
token expires (5 minutes), prepare it again.

If `schedule_appointment` answers `MCP_SCOPE_REQUIRED`, the connection is read-only. Tell the user
that booking needs a write-enabled connection, which a WindowEdge admin sets up as described in
the plugin README under "Turn on changes". Do not try another way to book.
