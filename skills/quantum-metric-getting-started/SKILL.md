---
name: quantum-metric-getting-started
description: Confirms the Quantum Metric subscription, workspace and timezone before any analysis, and fixes connection and access problems. Use when the user asks to "connect to Quantum Metric", "which workspace am I in", "switch workspace", or when a Quantum Metric tool returns an authorization error, an empty result or "no workspace set". Don't use for answering analytics questions (use quantum-metric-data-analysis) or explaining one session (use quantum-metric-session-review).
---

# Quantum Metric: getting started

Quantum Metric tools run as the signed-in user, with that user's role, permissions, workspace and timezone. The server keeps no state between calls.

## Available MCP Tools

| Tool | Mode | Purpose and required inputs |
| :--- | :--- | :--- |
| `get_context` | Read (`readOnlyHint: true`) | Returns the subscription, workspace and timezone in use. No inputs. |
| `list_workspaces` | Read | Lists the workspaces the user can use, with their IDs. |
| `search_skills` | Read | Finds the server's own guidance for a topic. Input: the topic. |
| `get_skill` | Read | Loads one guidance skill returned by `search_skills`. |
| `load_skill_resource` | Read | Loads a resource file that a guidance skill points to. |

## Standard Execution Workflow

1. **Prerequisite and ID resolution.** Call `get_context`. Tell the user in one line which subscription, workspace and timezone the session uses, before presenting any number.
2. **Scoped read query.** If the user needs another workspace, call `list_workspaces`, find the ID, and pass it as `workspace` on every later call. The workspace applies per call. To use another timezone, pass `timezone` on the tools that accept it.
3. **Read the server's guidance.** Before building a query, call `search_skills` with the user's topic. Load what it returns with `get_skill` and `load_skill_resource`, and follow it. The server's guidance is the source of truth for query construction, so never restate it from memory.
4. **Verification.** Confirm the workspace and timezone the next result reports match what the user asked for.

## Limits

- Requests are for targeted questions. For bulk data movement, point the user to the Quantum Metric API.
- Analytics queries are limited to 120 seconds. Narrow the time range or add filters rather than retrying the same query.
- A tool ignores parameters it does not define. Check each parameter name against the tool definition.

## When something fails

Read `references/troubleshooting.md` for the symptom and the fix. Do not read it when calls succeed.
