---
name: diagnose
description: Find out why a Nerdit app is failing, crashing, unhealthy or not answering, from its deploy phase, health probe and logs, and propose the fix. Use when the user says their Nerdit app is down, broken, returns 502, failed to deploy or "doesn't work".
---

# Diagnose a failing Nerdit app

1. **Find the app.** `nerdit_list_projects`, then `nerdit_get_project` with its `project_id`
   for live services and addresses. If the user names an app that is not a project, call
   `nerdit_list_nodes` and `nerdit_call_read_tool` → `list_services` on the machine.
2. **Diagnose.** `nerdit_diagnose_project` with `{project_id, service}` (`service` defaults to
   `web`). For a plain app: `nerdit_call_read_tool` → `diagnose_service` with `{name}`.
3. **Read logs when the diagnosis is not enough.** `nerdit_project_logs` with
   `source: "build"` for a failed build, `source: "runtime"` for a crash, `grep` for an error
   string, `tail` up to 1000.
4. **Explain and fix.** Start from `remediation.code` and `detail`. State the cause in one or
   two sentences, quote the log line that proves it, and propose the smallest fix: a code change
   in this repo, then the `deploy` skill again; a missing variable, which the user sets with
   `nerdit vars set <project> --secret --prompt KEY` (never ask for its value in chat); or a
   restart with `nerdit_call_write_tool` → `restart_service`. Ask before any write.

Not a failure: right after a hosted box wakes, its services come back one by one for up to a
couple of minutes. `machine_waking` means call again in 10 seconds.
