---
name: quantum-metric-data-analysis
description: Answers questions about funnels, conversion, errors, friction, traffic and trends from Quantum Metric data, using verified numbers. Use when the user asks "what are the top errors", "where are customers dropping out of checkout", "how many sessions", "compare this week to last week" or "show me the funnel". Don't use for explaining one session (use quantum-metric-session-review), creating a saved query (use quantum-metric-saved-queries) or running a workflow or monitoring agent (use quantum-metric-monitoring-workflows).
---

# Quantum Metric: data analysis

Work in this order. Each step prevents a plausible but wrong answer.

## Available MCP Tools

| Tool | Mode | Purpose and required inputs |
| :--- | :--- | :--- |
| `get_context` | Read (`readOnlyHint: true`) | Confirms subscription, workspace and timezone. Run first. |
| `list_saved_queries` / `run_saved_query` | Read | Lists saved queries, then runs one by ID. |
| `list_funnels` / `run_funnel` / `analyze_funnel_dropoff` | Read | Lists funnels, runs one, and finds where users drop out. |
| `list_dashboards` / `get_dashboard` / `execute_dashboard_card` | Read | Lists dashboards, reads one, runs one card. |
| `list_concepts` / `get_concept` | Read | Business concepts configured in the subscription. |
| `discover_page_candidates` | Read | Turns a page name into an analytics filter. |
| `list_events` / `get_event` / `search_event_values` | Read | Resolves event names and values. |
| `catalog_metrics` / `catalog_summary` | Read | Lists valid metrics and dimensions. |
| `run_query` | Read | Runs an ad-hoc analytics query for a question. |

## Standard Execution Workflow

1. **Prerequisite and ID resolution.** Call `get_context` if it has not run in this conversation. Then resolve every name to a real object. Pages: `discover_page_candidates`. Events: `list_events`, `get_event`, `search_event_values`. Metrics and dimensions: `catalog_metrics`, `catalog_summary`. Never guess an event, page or metric name; use the values the tools return.
2. **Scoped read query.** Reuse what exists before building a query: `list_saved_queries`, `list_funnels`, `list_dashboards` then `get_dashboard`, and `list_concepts`. Run a match with `run_saved_query`, `run_funnel` or `execute_dashboard_card`. Otherwise use `run_query` for ad-hoc questions, and `run_funnel` with `analyze_funnel_dropoff` for drop-off. Use explicit dates, and state the time range in the answer.
3. **Preflight validation.** All tools in this skill only read, so no confirmation is needed. Check that what ran matches what was asked: the right measure (count, rate, unique sessions or unique users), the right population (filters, segment, workspace) and the right period and timezone. If the question and the query differ, fix the query and run it again.
4. **Verification and report.** Report only numbers returned by a tool. Do not estimate, extrapolate or combine results from different populations. Say when a result is empty or limited. Include the links the tools return for dashboards, sessions and replays. To explain a number, hand off to `quantum-metric-session-review` with example sessions.
