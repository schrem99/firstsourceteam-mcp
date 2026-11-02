---
name: fst-program-report
description: Answer questions about a FirstSourceTeam outplacement program (engagement, placements, launch groups, trends, spend, invoices, plans) with the firstsourceteam MCP tools. Use when an HR admin asks how their outplacement program is doing, what it has cost, or how it compares to benchmarks.
---

# FirstSourceTeam program report

The `firstsourceteam` MCP server returns aggregate, read-only figures for the signed-in user's own organization.

## Pick the tool

| Question | Tool |
|---|---|
| How is the program doing? Engagement, placements, time to placement | `get_program_stats` (add a launch group or plan breakdown when asked) |
| Which launch groups exist? | `list_cohorts` |
| Month-by-month or quarterly counts | `get_program_trend` |
| How do we compare? | `get_benchmarks` |
| What have we paid, what is owed, seats left | `get_cost_summary` |
| Open or past invoices | `list_invoices` |
| Account setup, teammates, contract dates | `get_account_overview` |
| What each plan includes and costs | `get_plans` |

## Rules

- Report the `asOf` time and the organization name the tool returns.
- Counts of 1 to 4 come back as `null` with `suppressed: true`. Say they are hidden for privacy. Never estimate them or work them out from other numbers.
- A `null` figure that isn't suppressed means it isn't tracked. Say so; don't estimate.
- Money is given in cents with a formatted string. Quote the formatted string.
- There are no names, contact details or individual statuses. For "who" questions, point the user to the employer portal at https://firstsourceteam.com/portal.
- Use the definitions the tool returns (for example, activation rate = activated ÷ (added − declined)) when explaining a figure.
