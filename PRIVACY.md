# Privacy

The full WindowEdge privacy policy is at <https://windowedge.ai/privacy/>. This page summarizes
how this plugin handles data.

## Data collection

The plugin itself collects nothing. When you use it, Claude sends tool calls and their arguments
(for example a contact name to search for, or a date range) to the WindowEdge MCP server at
`https://mcp.windowedge.ai/mcp`. The server returns data from the WindowEdge business that
approved the connection.

## Use and storage

WindowEdge uses each tool call only to answer it. WindowEdge stores the business records that it
already holds for the dealer, plus audit records of tool calls and of every confirmed change.
OAuth tokens are opaque, and WindowEdge stores only their SHA-256 hashes.

## Third-party sharing

The plugin itself contacts only WindowEdge (`mcp.windowedge.ai` for tools and OAuth, and
`console.windowedge.ai` for sign-in and consent). Data that the tools return becomes part of your
Claude conversation. A change that you confirm can make WindowEdge send data through the
business's own configured integrations, for example a confirmation email or text to a homeowner,
a calendar update, a webhook or a CRM sync. WindowEdge's sub-processors are listed in its privacy
policy.

## Data retention

Retention follows the WindowEdge privacy policy and the dealer's agreement with WindowEdge. You
can revoke the connection at any time in the WindowEdge console under **Settings > MCP Keys**, or
by removing the connector in Claude.

## Contact

- Privacy: privacy@windowedge.ai
- Support: support@windowedge.ai
