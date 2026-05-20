# Triage Labels

This repo does not require a separate label database for imported skills. Use the tracker mapping below when a skill says "label", "triage", "ready", or "needs info".

| Imported role | Representation | Meaning |
| --- | --- | --- |
| `bug` | GitHub `bug` label | Something is broken |
| `enhancement` | GitHub `enhancement` label | New behavior or improvement |
| `needs-triage` | GitHub `needs-triage` label | Maintainer needs to evaluate |
| `needs-info` | GitHub `needs-info` label | Waiting on more information |
| `ready-for-agent` | GitHub `ready-for-agent` label | Brief is complete and can be picked up by an agent |
| `ready-for-human` | GitHub `ready-for-human` label | Needs human judgment, credentials, or manual operation |
| `wontfix` | GitHub `wontfix` label | Will not be actioned |

## Rules

- A triaged item should have exactly one category marker and one triage marker.
- `ready-for-agent` items need a durable agent brief, acceptance criteria, scope boundaries, and verification commands.
- `ready-for-human` items should name the exact human decision or credential needed.
- `needs-info` items should include the exact missing information in the task body, issue, or local note.
- Durable `wontfix` decisions should be recorded in an ADR or `.out-of-scope/` note when the reason is likely to be relitigated.
