---
name: support
description: Open a WindowEdge support request, check CS ticket progress and published updates, or review conversation feedback outcomes.
---

# WindowEdge support

Use the connected WindowEdge tools. Never invent a ticket, resolution, timestamp or delivery confirmation.

## Check a request

1. Call get_tenant_profile for the connected business and timezone.
2. For a named CS ticket call get_support_request. Otherwise call list_support_requests with scope "mine" by default. Use state "all" for older requests.
3. Use scope "organization" only when requested and permitted. Do not retry a denial with another identity or tenant.
4. Show the number, current status, what support understood, next step and published updates. Distinguish missing information from completed work.
5. Use the returned timezone. If timezoneSource is utc_fallback, say the business timezone is unavailable and label times UTC.
6. Link the returned consolePath under https://console.windowedge.ai. The user must sign in; a link never grants access.

## Open a request

1. Collect a short title, what happened, what the user expected, and an area. Never collect passwords, tokens or payment details.
2. Areas: SMS, Voice, Web Chat, Dealer Portal, Quoting, Canvassing, Project Module, Marketing, Other, Not Sure. Use corresponding tool-schema values; do not guess when unsure.
3. Call create_support_request. This prepares a request, but does not open it.
4. Show the complete confirmation recap, including whose identity files the request. Wait for the user's explicit yes.
5. Call confirm_action with that token and tool name. Report the returned CS number and link only after success.
6. For an uncertain result, check list_support_requests before preparing another request. Do not generate new requests to retry an uncertain send.

## Conversation feedback

Call list_support_feedback for thumbs-up/thumbs-down history and published outcomes from SMS, voice and web chat. Organization scope requires permission. Show recorded resolution dates and notes. Not Recorded does not mean resolved.

## Access

Reading requires support:read. Creating requires support:write plus confirmation. Existing connections retain their permissions. An administrator can reconnect with Support Read or create a write-enabled connection under Settings > MCP Keys. Standard sign-in stays read-only.

If tools are absent, explain that this connection does not expose them. Offer the signed-in console Support page or support@windowedge.ai. Never pretend a request was filed. MCP currently requires a business administrator. Other staff can use their personal Support page in WindowEdge.

These customer tools cannot assign tenants, publish internal notes, change ticket status, or declare a fix deployed.
