---
name: quantum-metric-saved-queries
description: Creates a saved query in Quantum Metric from a question the user has already answered, after validating it. Use when the user asks to "save this query", "create a saved query for top errors" or "save this as a saved search". Don't use for one-off questions (use quantum-metric-data-analysis) or for running an existing saved query.
---

# Quantum Metric: saved queries

Creating a saved query changes the subscription. Run the query first, confirm the details with the user, then save it.

## Available MCP Tools

| Tool | Mode | Purpose and required inputs |
| :--- | :--- | :--- |
| `list_saved_queries` | Read (`readOnlyHint: true`) | Lists existing saved queries, to avoid duplicates. |
| `run_query` | Read | Runs the query before it is saved. |
| `create_saved_query` | Write (`readOnlyHint: false`) | Saves a query. Required: `name` and `config`, where `config` is `{ "query": <query> }`. Optional: `description`, `tags`, `category`, `permission_scope`, `workspace`, `params`. |
| `run_saved_query` | Read | Runs a saved query by ID, to verify it. |

## Standard Execution Workflow

1. **Prerequisite and ID resolution.** Call `get_context` if it has not run in this conversation. Call `list_saved_queries` and tell the user if a saved query with the same purpose or name already exists.
2. **Scoped read query.** Build and run the query with `run_query` until it returns what the user expects. Follow the query guidance from `search_skills` and `get_skill`.
3. **Preflight validation before mutating.** Before calling `create_saved_query`, check that `name` and `config` are set, and ask the user for a name, a description, and who should be able to see it if any of these is missing. Use only the `permission_scope` and `category` values the tool's schema lists. State what will be saved and wait for the user to agree. A `config` without a `from` or without metrics, or a raw request envelope, is rejected: pass `{ "query": <query> }` only.
4. **Post-action verification.** After the save succeeds, read the result: the tool runs the query first, and reports a warning if it returned no rows or could not be validated. Confirm with `list_saved_queries` or `run_saved_query` using the returned ID, and give the user the saved query's name and ID.
