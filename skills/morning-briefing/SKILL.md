---
name: morning-briefing
description: Build the dealer owner's morning briefing from WindowEdge - today's appointments, yesterday's results, new leads and calls, conversations that need a human, overdue tasks and the pipeline. Use when the user asks for a morning briefing, daily brief, stand-up, "what's happening today" or "catch me up".
argument-hint: "[YYYY-MM-DD]"
---

# Morning briefing

Build the briefing in this order. If the user gave a date, use it as YYYY-MM-DD in the business
timezone. Otherwise use today.

1. Call `get_tenant_profile` for the business name and timezone.
2. Call `get_daily_brief` (pass the date if the user gave one).
3. Call `get_attention_queue` for what is waiting on someone.
4. Drill in only where the brief flags something:
   - unread homeowner replies: `list_sms_conversations` with `filter` = `unread`
   - flagged calls: `list_voice_calls` with `filter` = `flagged`
   - overdue tasks: `list_tasks` with `filter` = `overdue`

## Write the answer

Use five short sections, in this order:

1. **Today**: appointments, by rep and time.
2. **Yesterday**: appointments run, sold, cancelled and no-shows.
3. **Leads and calls**: new leads and what the AI agents handled.
4. **Needs a human**: flagged or unread conversations and overdue tasks, with names.
5. **Pipeline**: what moved and what is stuck.

Start each section with the one number that matters, then the names. Keep the whole briefing
under 250 words. Use only figures the tools returned. If a section was not available, say so in
one line.
