# Changelog

All notable changes to this plugin are listed here. The format follows Keep a Changelog and the versions follow Semantic Versioning.

## [1.1.0] - 2026-11-05

### Added

- Command `/saelyx:meeting`: find a meeting by topic, person or date and get its summary, decisions and action items.
- Command `/saelyx:calendar`: read today, tomorrow, this week or the next meeting from the connected calendar, and prepare the next meeting from earlier minutes and notes.
- Command `/saelyx:action-items`: list the action items raised in recent minutes, grouped by owner, with the meeting and date each one comes from.
- Command `/saelyx:weekly-recap`: a weekly summary of meetings, decisions and pending items, ready to paste.
- Command `/saelyx:find`: search minutes, notes, documents and memory, and use chat history or Ask Saelyx only when needed.

### Changed

- The directory icon is re-encoded without metadata (#1, merged before this release).

## [1.0.0] - 2026-11-03

First release.

### Added

- Connector entry for the Saelyx server (`https://mcp.saelyx.ai/mcp`, HTTP transport), the same server as the Saelyx connector in Claude's directory.
- Skill `meetings`: find a meeting, read its minutes, decisions and action items, and draft follow-ups and weekly recaps.
- Skill `agenda`: read the calendar (Outlook / Microsoft 365) for today, tomorrow, this week, next week or the next meeting, and prepare for an upcoming meeting.
- Skill `notes-memory-documents`: search and read notes, what Saelyx remembers, and the documents uploaded to Saelyx.
- Skill `search`: answer questions that could live in several places, search the Saelyx chat history, and use Ask Saelyx only when needed.
- Skill `troubleshooting`: connection, permission, plan and daily-limit problems, and which account, plan and usage are connected.
