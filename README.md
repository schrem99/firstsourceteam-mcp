<img src="assets/logo.png" alt="FirstSourceTeam" width="72">

# FirstSourceTeam MCP server

Ask your AI assistant about your FirstSourceTeam outplacement program in plain English: "How many people in the March launch group have started coaching?" or "What have we paid so far, and what's still owed?"

It's a hosted, read-only MCP server at `https://firstsourceteam.com/api/mcp`. You sign in with your FirstSourceTeam employer portal account (OAuth), and every answer is limited to your own organization. It returns totals, never names.

This repo holds the install configs and plugin manifests for Cursor, Claude Code and the MCP Registry. The server itself runs on firstsourceteam.com, so there's nothing to build or run locally.

## Who it's for

HR teams at employers that run an outplacement program with [FirstSourceTeam](https://firstsourceteam.com). You need:

- an employer portal account (admin or viewer) that has signed in to the portal at least once, and
- AI assistant access turned on for your organization. Ask hello@firstsourceteam.com to turn it on. Until it's on, sign-in works but every request is refused.

## Install

### Cursor

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/en/install-mcp?name=firstsourceteam&config=eyJ1cmwiOiJodHRwczovL2ZpcnN0c291cmNldGVhbS5jb20vYXBpL21jcCJ9)

Or add this to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "firstsourceteam": {
      "url": "https://firstsourceteam.com/api/mcp"
    }
  }
}
```

Cursor opens a sign-in page the first time a tool runs.

### Claude Code

```bash
claude mcp add --transport http firstsourceteam https://firstsourceteam.com/api/mcp
```

Or install it as a plugin, which also adds the `fst-program-report` skill:

```
/plugin marketplace add schrem99/firstsourceteam-mcp
/plugin install firstsourceteam@firstsourceteam
```

Run `/mcp` and choose **Authenticate** to sign in.

### Claude, ChatGPT, Gemini, Microsoft Copilot

Add `https://firstsourceteam.com/api/mcp` as a custom connector. Step-by-step guides: [firstsourceteam.com/docs/integrations/ai-assistants](https://firstsourceteam.com/docs/integrations/ai-assistants).

### Any other MCP client

| Setting | Value |
|---|---|
| URL | `https://firstsourceteam.com/api/mcp` |
| Transport | Streamable HTTP |
| Auth | OAuth 2.1 (dynamic client registration or client ID metadata document), discovered from `/.well-known/oauth-protected-resource/api/mcp` |

## Tools

All eight tools are read-only and return aggregates for your organization, plus the time the figures were read and definitions of each figure.

| Tool | Ask it | Returns |
|---|---|---|
| `get_program_stats` | "How's engagement on the March group?" | The funnel (added, invited, activated, placed, declined, expired), activation, placement and decline rates, median days to first contact and to placement. Optionally by launch group or plan. |
| `list_cohorts` | "Which launch groups do we have?" | Each launch group's name and size. Groups under 5 people are combined. |
| `get_program_trend` | "How many people started each month this year?" | Counts per month or quarter for the last 3 to 24 months. |
| `get_benchmarks` | "How does our engagement compare?" | FirstSourceTeam's published figures with sources, next to your own rates. |
| `get_cost_summary` | "What have we paid, and what's still owed?" | Paid, refunded, outstanding and overdue amounts, seats bought, used and remaining, contract dates. |
| `list_invoices` | "Show our open invoices." | Invoice number, dates, status and amounts, linked to the portal billing page. |
| `get_account_overview` | "Who's on our account?" (counts only) | Setup status, default plan, your role, teammate and pending-invitation counts, contract dates. |
| `get_plans` | "What does the Leader plan include?" | Each plan's price, coaching hours, length and what's included. |

Full reference: [firstsourceteam.com/docs/integrations/mcp-tools](https://firstsourceteam.com/docs/integrations/mcp-tools).

## Privacy

- No tool returns a participant's name, email, phone, job title, ID, notes or individual status. "Who" questions are pointed back to the employer portal.
- Counts of 1 to 4 people are hidden (`null`, `suppressed: true`), and small groups are combined, so no number points to a person.
- Which organization you see comes from your portal account, never from what you or the assistant type.
- There are no write tools. It can't add participants, send invitations or move money.
- Every request is logged and appears in your organization's portal audit log.
- Limits: 30 requests a minute per person, 300 an hour per organization.

Details: [firstsourceteam.com/docs/integrations/mcp-privacy](https://firstsourceteam.com/docs/integrations/mcp-privacy).

## Support

hello@firstsourceteam.com

## License

The configs and docs in this repo are MIT licensed. Use of the FirstSourceTeam service is covered by its [privacy policy](https://firstsourceteam.com/legal) and your contract.
