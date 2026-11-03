---
name: troubleshooting
description: "Diagnose problems with the Saelyx connector: tools not available, a missing permission, a calendar or service that is not connected, a plan or daily-limit message, or an expired sign-in. Also answers which Saelyx account is connected and what the user's plan and usage are. Use when a Saelyx tool fails or when the user asks about their Saelyx connection or account."
---

# Saelyx connection and account

Use this when a Saelyx tool is missing or fails, or when the user asks about their Saelyx connection, account, plan or usage.

## No Saelyx tools are available

The connector is not connected. Do not answer from guesses. Tell the user how to connect it:

- Claude on the web, desktop or mobile, and Cowork: open the Saelyx plugin's Connectors tab (Customize > Plugins), or Customize > Connectors. Add Saelyx if it is not there, then select Connect. Claude opens saelyx.ai, where the user signs in and ticks what Claude may read.
- Claude Code: run `/mcp`, select `saelyx` and choose Authenticate. A browser window opens on the same Saelyx screen. Without the plugin, add the server first with `claude mcp add --transport http saelyx https://mcp.saelyx.ai/mcp`.

If the Saelyx sign-in says the account is available by invitation only, the user needs an invitation. They can request one at saelyx.ai.

## A permission is missing

Each tool needs one permission, which the user grants on the Saelyx consent screen at https://saelyx.ai/conectar/ (currently in Spanish). Two of them come unticked.

| Tools | Permission to tick | Ticked by default |
|---|---|---|
| `saelyx_list_meetings`, `saelyx_search_meetings`, `saelyx_get_meeting` | Read your meetings and minutes | yes |
| `saelyx_get_agenda` | Read your agenda | yes |
| `saelyx_list_memories`, `saelyx_search_memories` | Read what Saelyx remembers about you | yes |
| `saelyx_list_knowledge_bases`, `saelyx_search_knowledge` | Read your knowledge bases | yes |
| `saelyx_list_notes`, `saelyx_get_note` | Read your notes | yes |
| `saelyx_search_conversation` | Read your chat history with Saelyx | no |
| `saelyx_ask` | Ask Saelyx | no |
| `saelyx_whoami`, `saelyx_get_account_status` | See which Saelyx account is connected | yes |
| `saelyx_ping` | none | not applicable |

Tell the user which permission is missing and that they need to reconnect Saelyx and tick it. Keep working with the sources you can read. Never push the user to grant a permission they do not want to give.

## Other messages

| What the tool says | What it means and what to do |
|---|---|
| That service is not connected to this Saelyx account. | The service is not linked in Saelyx. For the agenda, the user connects an Outlook / Microsoft 365 calendar in the Saelyx app, under Connections. Do not retry. |
| This needs a paid Saelyx plan. | The user's Saelyx plan does not include this. Say so; their plan is shown in their Saelyx account. |
| Too many requests. Wait a moment and try again. | Wait, then retry once. |
| The daily memory search limit is reached; try again tomorrow. | The daily quota for memory searches is used up. Do not retry today. Browse with `saelyx_list_memories` instead. |
| The daily Saelyx chat limit is reached; try again tomorrow. | The user's daily Saelyx chat messages are used up, so Ask Saelyx is unavailable until tomorrow. Do not retry today. |
| Saelyx did not accept this connection's credentials. Reconnect the connector and try again. | The sign-in expired or was revoked. The user reconnects Saelyx. |
| The result is too large. Narrow the request with a smaller limit or more filters. | Repeat with a smaller `limit`, a filter or a narrower window. |
| The request ran out of time. Try again with a narrower request. | Retry once, narrower. |
| Saelyx is temporarily unavailable. Try again in a moment. | Retry once after a pause. If it fails again, tell the user and stop. |
| The requested item was not found. | Check that the id was copied exactly from a list or search result. |
| Saelyx rejected the request as invalid. | Check the arguments against the tool's limits. |
| Saelyx returned an unexpected response. | Retry once. If it happens again, tell the user it is a problem on the Saelyx side and suggest writing to info@saelyx.ai. |
| Unexpected error in the connector. | Same as above: retry once, then tell the user and stop. |

Retry at most once, and only for temporary errors. Never loop.

## Which account, plan and usage

- `saelyx_whoami` returns the email and the Saelyx id of the connected account, and nothing else. Use it when the user asks which account is connected.
- `saelyx_get_account_status` returns the plan, the subscription status, the user's name, this month's usage against the limit (`usage`), the documents allowed per cycle, and whether Odoo and Jira are linked (two true or false values). `usage` or `integrations` can be null when that part could not be read. Odoo and Jira content is not available through this connector.
- `saelyx_ping` checks that the connector is reachable and reads no account data. Use it only to tell a connector problem from a mistake in your request.

Share the email or the id only when the user asked for them.

## Removing access

To stop Claude from reading Saelyx altogether, the user opens their Saelyx account on the web and goes to Connectors (the page is in Spanish). They select "Quitar acceso" next to Claude and confirm with "¿Seguro? Quitar". Saelyx stops serving that connection within a minute. Removing Saelyx from Claude's side alone does not cancel the permissions in Saelyx, so they should do both.
