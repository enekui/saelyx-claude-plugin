# Saelyx for Claude

Let Claude read your Saelyx meeting minutes, calendar, memory, documents, notes and chat history. Read-only, permission by permission, and only for as long as you want.

[Saelyx](https://saelyx.ai) is a personal assistant. It listens to your meetings, writes the minutes, remembers what matters to you and keeps the documents and notes you give it. This plugin connects Claude to that data and teaches Claude how to use it well.

**Status.** The plugin is entering Claude's directory. Until it is listed, you can use it straight from this repository: see [Try it from this repository](#try-it-from-this-repository).

## What is inside

- **The Saelyx connector.** `.mcp.json` points to `https://mcp.saelyx.ai/mcp`, the same server as the Saelyx connector in Claude's directory. If you have both, Claude still sees one set of tools.
- **Five skills** that tell Claude when to reach for Saelyx and how to do it well.
- **Five commands for Claude Code.** `/saelyx:meeting`, `/saelyx:agenda`, `/saelyx:action-items`, `/saelyx:weekly-recap` and `/saelyx:search` run the most common jobs with one short command. They are plain text instructions, like the skills.
- **Nothing else.** No agents, hooks or scripts. The plugin runs no code on your machine.

| Skill | Ask things like | Tools it uses |
|---|---|---|
| `meetings` | "What did we decide in yesterday's call?" "Draft the follow-up." "Recap my week." | `saelyx_list_meetings`, `saelyx_search_meetings`, `saelyx_get_meeting` |
| `agenda` | "What is my next meeting?" "What is on tomorrow?" "Help me prepare for it." | `saelyx_get_agenda`, plus the meeting, memory and document tools for context |
| `notes-memory-documents` | "Find my note about the lease." "What does Saelyx remember about my landlord?" "What does the contract I uploaded say about notice periods?" | `saelyx_list_notes`, `saelyx_get_note`, `saelyx_list_memories`, `saelyx_search_memories`, `saelyx_list_knowledge_bases`, `saelyx_search_knowledge` |
| `search` | "What do you know about the budget review?" "What did I tell Saelyx about my schedule?" | All of the above, plus `saelyx_search_conversation` and `saelyx_ask` |
| `troubleshooting` | "Is Saelyx connected?" "Which account is this?" A permission or limit message. | `saelyx_ping`, `saelyx_whoami`, `saelyx_get_account_status` |

In Claude Code the skills appear as `/saelyx:meetings`, `/saelyx:notes-memory-documents` and so on. In chat, type `/` and pick one, or just ask: Claude loads the right skill when your request matches it.

## What Claude can read

The connector has 15 tools, all read-only. Each one needs a permission that you grant on Saelyx's consent screen. The screen is currently in Spanish; the labels below are translations.

| Permission | Ticked by default | Tools |
|---|---|---|
| See which Saelyx account is connected | yes | `saelyx_whoami`, `saelyx_get_account_status` |
| Read your meetings and minutes | yes | `saelyx_list_meetings`, `saelyx_search_meetings`, `saelyx_get_meeting` |
| Read your agenda | yes | `saelyx_get_agenda` |
| Read what Saelyx remembers about you | yes | `saelyx_list_memories`, `saelyx_search_memories` |
| Read your knowledge bases | yes | `saelyx_list_knowledge_bases`, `saelyx_search_knowledge` |
| Read your notes | yes | `saelyx_list_notes`, `saelyx_get_note` |
| Read your chat history with Saelyx | no | `saelyx_search_conversation` |
| Ask Saelyx | no | `saelyx_ask` |
| None | not applicable | `saelyx_ping` |

What the tools return:

- **Meetings.** The minutes: summary, topics, decisions, action items, open questions and highlights. Never the raw transcript.
- **Agenda.** Events from the Outlook / Microsoft 365 calendar you connected to Saelyx, for today, tomorrow, this week, next week or the next meeting.
- **Memory.** Facts Saelyx has learned about you. Never photos, faces or voice data.
- **Documents.** Passages from the files you uploaded to Saelyx, with the file name.
- **Notes.** Your notes, synced between your devices.
- **Chat history and Ask Saelyx.** Your conversation with Saelyx, and answers from Saelyx itself. Each Ask Saelyx question uses one of your daily Saelyx chat messages.

Not available through this connector: email, tasks, files other than the documents you uploaded to Saelyx, and anything that creates, changes or deletes data. Odoo and Jira are a separate Saelyx connector and are not part of this plugin.

The connector also offers five starter prompts: Follow up on a meeting, Weekly digest, Prepare the next meeting, What do you know about a topic, and Open work items.

## Install

You need a Saelyx account. Access to Saelyx is by invitation; you can request it at [saelyx.ai](https://saelyx.ai).

### Claude on the web, desktop and mobile, and Cowork

Once the plugin is listed in the directory (available on the Pro, Max, Ultra, Team and Enterprise plans):

1. Open **Customize > Plugins**, go to **Discover**, select Saelyx and choose **Add**.
2. Open the plugin's **Connectors** tab, add Saelyx if it is not there yet, and select **Connect**.
3. Claude sends you to saelyx.ai. Sign in, tick what Claude may read, and select **Allow**.

The skills load as soon as the plugin is added. They cannot reach Saelyx until the connector shows **Connected**.

### Claude Code

Once the plugin is listed in the directory:

1. Run `/plugin directory` (Claude Code 2.1.287 or later), select Saelyx and install it. A plugin you added on claude.ai also arrives by itself at the next session start; run `/reload-plugins` to load it.
2. Run `/mcp`, select `saelyx` and choose **Authenticate**. A browser window opens on the same Saelyx consent screen.

Once installed, the plugin also adds five commands to Claude Code. They are shortcuts for the most common jobs, and they only read Saelyx:

| Command | What it does |
|---|---|
| `/saelyx:meeting <what to look for>` | Finds a meeting by topic, person or date and returns its summary, decisions and action items. |
| `/saelyx:agenda [today\|tomorrow\|week\|next]` | Reads your calendar and prepares the next meeting from earlier minutes and notes. |
| `/saelyx:action-items [person or period]` | Lists the action items raised in recent minutes, grouped by owner. |
| `/saelyx:weekly-recap [week]` | Gives a summary of the week's meetings, decisions and pending items, ready to paste. |
| `/saelyx:search <question>` | Searches minutes, notes, documents and memory. Chat history and Ask Saelyx only when needed; Ask Saelyx uses one of your daily chat messages. |

### Try it from this repository

This repository is also a one-plugin marketplace, so Claude Code can install the plugin straight from it. In a session (Claude Code 2.1.275 or later):

```text
/plugin install saelyx --marketplace enekui/saelyx-claude-plugin
```

Claude Code asks you to confirm the source, adds the marketplace, then opens the plugin's details so you can choose a scope and install. From your shell (2.1.292 or later) one command installs it without asking:

```bash
claude plugin install saelyx --marketplace enekui/saelyx-claude-plugin
```

To load a local copy instead, without installing it:

```bash
git clone https://github.com/enekui/saelyx-claude-plugin.git
claude --plugin-dir ./saelyx-claude-plugin
```

Then run `/mcp`, select `saelyx` and choose **Authenticate**, as above.

### Only the connector, without the skills

- In Claude: find Saelyx in the [directory](https://claude.ai/directory/saelyx) or under **Customize > Connectors**, and select **Connect**.
- In Claude Code, add the server by hand, then run `/mcp`, select `saelyx` and choose **Authenticate**:

```bash
claude mcp add --transport http saelyx https://mcp.saelyx.ai/mcp
```

With the plugin installed you do not need that command.

## Privacy and control

- **You choose, checkbox by checkbox.** On the consent screen at [saelyx.ai/conectar](https://saelyx.ai/conectar/) you tick what Claude may read. Nothing is shared until you approve. Your chat history and Ask Saelyx come unticked.
- **Read-only.** Claude cannot create, change, send or delete anything in Saelyx.
- **What this plugin does.** It ships text instructions (the skills and the commands) and one connector entry. It stores nothing, collects nothing and runs nothing on your machine. Data moves only when Claude calls a Saelyx tool, and only between Claude and `mcp.saelyx.ai`, with your own Saelyx sign-in. For the agenda, Saelyx reads your upcoming events from the Outlook / Microsoft 365 calendar you connected to it. Your calendar credentials stay in Saelyx, and the token Claude uses is never passed on.
- **What Claude receives.** The results of a tool call become part of your conversation in Claude and stay there.
- **Never available.** Raw meeting transcripts; photos, faces, voice or other biometric data; your billing and security settings; the credentials of the services you connected to Saelyx.
- **Access history.** Saelyx keeps a record of each connector access (which connection, tool and permission, when, how long it took and how big the answer was) for up to 90 days, without the content. The details are in section 13 of the [privacy policy](https://saelyx.ai/en/privacy/).
- **Revoke at any time.** In your Saelyx account on the web, open **Connectors** (the page is in Spanish), select **Quitar acceso** next to Claude and confirm with **¿Seguro? Quitar**. Saelyx stops serving that connection within a minute. Removing Saelyx from Claude's side alone does not cancel the permissions in Saelyx, so revoke there too. Revoking does not delete what Claude already received.

## Support

- Questions or problems: [info@saelyx.ai](mailto:info@saelyx.ai) or the [support page](https://saelyx.ai/en/support/).
- Connector guide: [saelyx.ai/en/connect/claude](https://saelyx.ai/en/connect/claude/). Documentation: [saelyx.ai/en/docs/connectors](https://saelyx.ai/en/docs/connectors/).
- [Privacy policy](https://saelyx.ai/en/privacy/) and [terms](https://saelyx.ai/en/terms/).
- Security issues: write to the same address. The disclosure policy is at [saelyx.ai/.well-known/security.txt](https://saelyx.ai/.well-known/security.txt).

## License

[MIT](LICENSE). Changes are listed in the [changelog](CHANGELOG.md).

## En español

**Saelyx para Claude** conecta Claude con tu cuenta de Saelyx para que pueda leer tus actas de reuniones, tu agenda (Outlook / Microsoft 365), lo que Saelyx recuerda de ti, tus documentos, tus notas y tu historial de chat con Saelyx. Solo lectura: no crea, cambia ni borra nada. Incluye el conector de Saelyx y cinco skills que enseñan a Claude a usarlo.

- **Instalar.** Sigue los pasos de la sección Install; las pantallas de Claude pueden salir en tu idioma. Hace falta una cuenta de Saelyx. Mientras el plugin entra en el directorio de Claude, puedes usarlo desde este repositorio.
- **Comandos.** En Claude Code, el plugin añade cinco comandos de barra: `/saelyx:meeting`, `/saelyx:agenda`, `/saelyx:action-items`, `/saelyx:weekly-recap` y `/saelyx:search`. Hacen los trabajos más comunes con una sola orden, son de solo lectura y no ejecutan código.
- **Permisos.** Tú eliges, casilla a casilla, qué puede leer Claude en la pantalla de permisos de Saelyx ([saelyx.ai/conectar](https://saelyx.ai/conectar/)). Tu historial de chat y preguntar a Saelyx vienen sin marcar.
- **Revocar.** En tu cuenta de Saelyx en la web, abre **Conectores**, pulsa **Quitar acceso** junto a Claude y confirma con **¿Seguro? Quitar**.
- **Ayuda.** [info@saelyx.ai](mailto:info@saelyx.ai).
