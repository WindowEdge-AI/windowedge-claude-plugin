![WindowEdge](assets/windowedge-mark.png)

# WindowEdge for Claude

Run your window and door dealership from Claude. This plugin connects Claude to your live
[WindowEdge](https://windowedge.ai) business data and adds skills that turn that data into the
answers an owner, sales manager or marketing lead needs every day: a morning briefing, rep
scorecards, marketing reviews, a full customer history, at-risk customers, diagnostics for "why
did this number move", executive briefs, and appointment booking that never happens without your
confirmation.

You need a WindowEdge account. A WindowEdge **administrator** of your business must approve the
connection.

## What you get

**The WindowEdge connector** (`https://mcp.windowedge.ai/mcp`). Claude can read your contacts
(homeowners), appointments and availability, sales pipeline, quotes and invoices, marketing leads
and spend, tasks, and the conversations your AI voice, SMS and web chat agents had. It can also
prepare changes: create, update or delete a contact, book, reschedule or assign an appointment,
set an appointment result, and record ad spend.

**Skills**

| Skill | Ask Claude |
|---|---|
| `windowedge` | Loads automatically. Teaches Claude which tool answers which question, the business timezone rule, and the confirm-before-change rule. |
| `morning-briefing` | "Give me my morning briefing." |
| `rep-scorecard` | "How is Carol doing over the last 60 days?" |
| `marketing-review` | "Which marketing channels are wasting money?" |
| `customer-360` | "Tell me everything about the Hartmann family." |
| `at-risk-customers` | "Who is unhappy or going cold?" |
| `diagnose-change` | "Why did our booking rate drop this week?" |
| `executive-brief` | "Give me the weekly owner brief." |
| `book-appointment` | "Book Zane Hartmann for a consultation Thursday afternoon with Carol." |

In Claude Code, run a skill directly as `/windowedge:morning-briefing` (and so on).

## Install

**claude.ai, Claude Desktop and Cowork:** add WindowEdge from the
[Claude directory](https://claude.ai/directory).

**Claude Code:**

```bash
claude plugin marketplace add WindowEdge-AI/windowedge-claude-plugin
claude plugin install windowedge@windowedge
```

## Connect your account

1. Start Claude and use any WindowEdge skill, or open `/mcp` in Claude Code and select
   `windowedge`.
2. Claude opens the WindowEdge sign-in page. Sign in with your WindowEdge login.
3. Read the access that the consent screen asks for, then select **Allow Access**. Only a
   WindowEdge administrator can approve.
4. Ask "Give me my morning briefing."

The standard sign-in gives the connection **read access** to your business. Write tools answer
"missing scope" until you turn on changes (below). To disconnect, remove the connector
in Claude, or revoke it in the WindowEdge console under **Settings > MCP Keys**. Full setup help:
[windowedge.ai/connect-to-claude](https://windowedge.ai/connect-to-claude/).

## Turn on changes (optional)

The standard sign-in never grants write access. To let Claude prepare changes (contacts,
appointments, ad spend), a WindowEdge administrator creates a write-enabled connection in the
WindowEdge console under **Settings > MCP Keys**, and chooses only the write scopes the business
wants (`customers:write`, `appointments:write`, `marketing:write`, `support:write`):

- **claude.ai and Claude Desktop:** create an **OAuth client** with the redirect URI
  `https://claude.ai/api/mcp/auth_callback`. In Claude, add a custom connector for
  `https://mcp.windowedge.ai/mcp` and enter the Client ID and Client Secret under
  **Advanced settings**.
- **Claude Code:** create an **API key**, then run:

  ```bash
  claude mcp add --transport http windowedge-write https://mcp.windowedge.ai/mcp --header "Authorization: Bearer <your WindowEdge API key>"
  ```

Every change still needs your confirmation, as described next.

## Safety: every change needs your yes

No write tool changes anything on its first call. It returns a recap of the change and its side
effects (for example, a confirmation text or email to the homeowner, or a CRM sync). The change
happens only after you confirm and Claude calls `confirm_action` with a one-time token that
expires after 5 minutes. Deleting a contact also needs you to type `delete contact` yourself.

## What this plugin runs, sends and stores

- **Runs:** nothing on your computer. The plugin contains no hooks, scripts or local servers.
  It is skills (Markdown instructions) plus one remote MCP server address.
- **Sends:** Claude sends tool calls and their arguments to the WindowEdge MCP server at
  `https://mcp.windowedge.ai/mcp`. The server returns your business data to Claude, so it becomes
  part of your Claude conversation. The plugin itself contacts no other service.
- **Authentication:** OAuth 2.0 with PKCE, through WindowEdge's endpoints on the same host
  (`/.well-known/oauth-*`, `/oauth/register`, `/oauth/authorize`, `/oauth/token`). Sign-in and
  consent happen on the WindowEdge console (`console.windowedge.ai`). Claude stores the token;
  the plugin holds no credentials. Access tokens last one hour and refresh automatically.
- **What a confirmed change can trigger:** WindowEdge applies the change with the business's own
  configured automations. For example, booking an appointment can send a confirmation email or
  text to the homeowner, update connected calendars, fire the business's webhooks, and sync to
  its CRM. The confirmation recap lists these side effects before you say yes.
- **Data access:** only the WindowEdge business that approved the connection, and only within the
  scopes that the connection holds.
- **Stores:** the plugin stores nothing. WindowEdge keeps its normal audit records of tool calls
  and of every change, as its [privacy policy](https://windowedge.ai/privacy/) describes.

## Privacy policy

WindowEdge's privacy policy covers data collection, use, storage, third-party sharing, retention
and contact details: [windowedge.ai/privacy](https://windowedge.ai/privacy/). See also
[PRIVACY.md](PRIVACY.md). Privacy questions: privacy@windowedge.ai.

## Support

Ask Claude to list your requests, show a CS ticket's published progress, or review conversation feedback outcomes. Support Read permission is required. An administrator can request the organization view. Opening a request requires Support Write and your confirmation. Existing connections keep their permissions; reconnect or create an explicitly scoped connection to add access. These tools cannot expose internal notes or change ticket status or tenant assignment.

Use `/windowedge:support` in Claude Code. If support tools are unavailable on your connection, use the signed-in WindowEdge Support page or email below.


- Email: support@windowedge.ai
- Help: [windowedge.ai/support](https://windowedge.ai/support/)
- Issues with this plugin: [GitHub issues](https://github.com/WindowEdge-AI/windowedge-claude-plugin/issues)

## License

[MIT](LICENSE)
