---
name: quantum-metric-monitoring-workflows
description: Reviews and runs Quantum Metric monitoring agents and workflows, and reads their results. Use when the user asks to "run the monitoring agent", "check the latest monitoring run", "run this workflow" or "show the workflow report". Don't use for ad-hoc analytics questions (use quantum-metric-data-analysis).
---

# Quantum Metric: monitoring agents and workflows

Reading is always safe. Running a monitoring agent or a workflow changes the subscription, and a workflow can send email, Slack messages and webhooks.

## Available MCP Tools

| Tool | Mode | Purpose and required inputs |
| :--- | :--- | :--- |
| `list_monitoring_agents` / `get_monitoring_agent` | Read (`readOnlyHint: true`) | Lists agents and reads one agent's configuration. |
| `list_monitoring_agent_runs` / `get_monitoring_run_results` | Read | Past runs, and the results of one run by `run_id`. |
| `run_monitoring_agent` | Write (`readOnlyHint: false`) | Starts an agent now. Required: `agent_id`. Optional: `time_period`, `timezone`. Returns a `run_id`. |
| `list_workflows` / `get_workflow` | Read | Lists workflows and reads one workflow's steps. |
| `list_workflow_runs` / `get_workflow_run` / `get_workflow_report` | Read | Past runs, one run's result, and a run's report. |
| `execute_workflow` | Write (`readOnlyHint: false`) | Runs a workflow now and waits for it. Required: `workflow_id`. Optional: `timezone`. |

## Standard Execution Workflow

1. **Prerequisite and ID resolution.** Call `get_context` if it has not run in this conversation. Resolve the agent or workflow by name with `list_monitoring_agents` or `list_workflows`, and use the ID the tool returns. Never guess an ID.
2. **Scoped read query.** Read the agent with `get_monitoring_agent`, or the workflow with `get_workflow`. For results that already exist, use `list_monitoring_agent_runs` and `get_monitoring_run_results`, or `list_workflow_runs` and `get_workflow_run`, and stop there if that answers the question.
3. **Preflight validation before mutating.** Before `run_monitoring_agent` or `execute_workflow`, tell the user which agent or workflow will run and, for a workflow, what it delivers: its action steps and an automation's delivery actions send their email, Slack messages and webhooks. A report workflow's deliveries are not sent from here. Wait for the user to confirm.
4. **Post-action verification.** A monitoring run starts as "queued", then "running": poll `get_monitoring_run_results` with the `run_id` while it is either, no more than once every few seconds, and report the final status. `execute_workflow` returns the finished run: report its status and results, and use `get_workflow_report` to read the report. Say plainly if a run failed.
