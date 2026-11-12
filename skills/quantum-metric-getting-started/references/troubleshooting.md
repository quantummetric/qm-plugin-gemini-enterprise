# Quantum Metric troubleshooting

| Symptom | What to do |
| :--- | :--- |
| Connected, but no tools appear | Restart the client, then confirm the server URL is `https://api.quantummetric.com/mcp`. |
| 401: access token expired, rejected or no longer valid | Sign in to Quantum Metric again from the client. |
| 403 "Session expired or unauthorized" on every call | The subscription is one the signed-in account cannot access. Check the subscription name and sign in with the account that belongs to it. |
| "You do not have access to this feature" on specific tools | The user's role does not include that feature. Ask a Quantum Metric admin to review the role. |
| "Error: no workspace set" | Call `list_workspaces` and pass a workspace ID as `workspace`. |
| Query times out | Narrow the time range, add filters, or run a saved query. |
| Query returns no data | Check the workspace, the time range and the event name. |
