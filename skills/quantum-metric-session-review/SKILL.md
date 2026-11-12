---
name: quantum-metric-session-review
description: Investigates one Quantum Metric session, or a small set of sessions, to explain what a visitor experienced, with a replay link. Use when the user asks to "summarise this session", "what happened in this session", "show me a session with this error", or provides a session ID or replay link. Don't use for sizing how many users were affected (use quantum-metric-data-analysis).
---

# Quantum Metric: session review

Sessions explain a number. They do not produce one.

## Available MCP Tools

| Tool | Mode | Purpose and required inputs |
| :--- | :--- | :--- |
| `get_session_summary` | Read (`readOnlyHint: true`) | Journey, errors and friction for one session. Input: session ID or URL. |
| `get_user_summary` | Read | The same for a user across sessions. |
| `get_session_hits` | Read | Page-level and event-level detail around a moment. |
| `get_replay_screenshot` | Read | The screen at a moment in the session. |
| `get_session_replay_urls` | Read | Replay links to share. Needs at least one session ID. |
| `run_query` | Read | Finds example sessions that match a condition. |

## Standard Execution Workflow

1. **Prerequisite and ID resolution.** Call `get_context` if it has not run in this conversation. If the user has no session ID, find one with `run_query` (for example, a session that hit a named error) and use the ID it returns. Never guess a session ID.
2. **Scoped read query.** Call `get_session_summary` with the session ID or URL. For a user rather than a session, call `get_user_summary`. Go deeper only where the summary points: `get_session_hits` for detail around a moment, `get_replay_screenshot` for the screen at a moment.
3. **Preflight validation.** All tools in this skill only read, so no confirmation is needed. Check that the session is the one asked about, and that its time falls in the period the user cares about.
4. **Verification and report.** Describe the sequence of events and the point where the experience broke. Quote errors and values exactly as the tools return them. If a tool call fails or the data is unavailable, say so; never reconstruct events. Include the replay link (`get_session_replay_urls`) for every session discussed.

## Looking across sessions

For a pattern, review three to five sessions from the affected group. Report what they have in common and how many were reviewed. Use `quantum-metric-data-analysis` to size the pattern across all sessions.
