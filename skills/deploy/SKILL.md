---
name: deploy
description: Deploy the current Git repository (or a project with a nerdit.toml) to the user's Nerdit machine or Nerdit box, wait until it is healthy, and return a private URL. Use when the user asks to deploy, ship, host, publish or "put online" this repo or app with Nerdit.
---

# Deploy this repo to Nerdit

Uses the Nerdit MCP server. Machine tools are reached through `nerdit_call_read_tool` (reads)
and `nerdit_call_write_tool` (changes), each with `{node_id, tool_name, arguments}`.

1. **Pick the machine.** Call `nerdit_list_nodes`. None: tell the user to link one
   (`curl -fsSL get.nerdit.ai | sh`) or start a box at https://app.nerdit.ai/box, then stop.
   Several: ask which one. Keep its `node_id`.
2. **Find the source.** Read `git remote get-url origin` and the current branch. Deploys clone
   over `https://` (github.com by default), so an SSH remote becomes its `https://` form. The
   branch must be pushed: if `git status` shows unpushed commits, say so and ask before going on.
   Private repo: pass `token_ref: "${github.installation}"` (Nerdit GitHub App), never a token.
3. **Choose the path.**
   - Root `nerdit.toml` with `[services.*]` (a project declaration): if the project already
     exists (`nerdit_list_projects`), use `nerdit_apply_project` with its `project_id`,
     `repo_url`, `ref`, first with `dry_run: true`. Otherwise `nerdit_call_write_tool` →
     `apply_project` with `{project: <[project].name>, repo_url, ref, dry_run: true}`.
   - Anything else: `nerdit_call_write_tool` → `deploy_git` with
     `{repo_url, name, ref, dry_run: true}`, plus `subdir` when the app is a folder of a monorepo.
     `name` is short kebab-case, from the repo name.
4. **Read the dry run.** Show the plan in a few lines. `waiting_for_variables` deploys nothing:
   list the `missing` names and ask the user to run, on their side,
   `nerdit vars set <project> --secret --prompt KEY`. Never ask for secret values in chat.
5. **Deploy.** Same call without `dry_run`. Keep `last_deploy.version` from the answer.
6. **Wait.** `nerdit_call_read_tool` → `wait_for_service` with `{name, version, timeout: 300}`
   (once per service `label` after an apply). `failed`: read `diagnosis.remediation` and follow
   the `diagnose` skill. `timeout`: call it again.
7. **Get a URL.** `nerdit_call_write_tool` → `share_service` with `{name, access: "private"}`.
   Give the user the returned `url`: it opens for them once signed in to Nerdit. Do not make an
   app public unless the user asks for it in this conversation.

A box that was asleep can answer `machine_waking`: wait 10 seconds and call again.
