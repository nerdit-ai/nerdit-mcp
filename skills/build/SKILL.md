---
name: build
description: Build a new app, agent or bot that runs on Nerdit, using the platform's building blocks (AI model gateway, scheduled jobs, webhooks, secrets, sharing) instead of hand-rolled infrastructure, then deploy it. Use when the user asks to build, create or automate something, such as "build me an app / an agent / a bot that watches, monitors, enriches, summarizes or alerts", a recurring task, or an automation to host on Nerdit.
---

# Build an app or agent on Nerdit

The machine's guides are the source of truth for `nerdit.toml` keys, headers and errors: they
match the engine version on that machine. This skill never states them; follow the guide.

1. **Pick the machine.** `nerdit_list_nodes`, as in the `deploy` skill (none: send the user to
   https://app.nerdit.ai/machines or https://app.nerdit.ai/box and stop). Keep the `node_id`.
2. **Read the guides first.** `nerdit_list_tools` with `{node_id, name: "get_guide"}`. If it returns the tool:
   `nerdit_call_read_tool` → `get_guide` with `{topic: "index"}`, then read the guide the index
   points to for this request (and any it links). Read them before writing code.
   Empty list: the engine on that machine predates guides. Say so and offer to update it
   from https://app.nerdit.ai/machines. Until then, only build a plain web app (with the
   `deploy` skill); do not guess the syntax for models, schedules or webhooks.
3. **Use the blocks, not your own plumbing.** A model call goes through the Nerdit AI gateway
   binding the guide describes, never a provider key in a variable. A recurring task is a
   scheduled job; an inbound event is a webhook. Prefer a scheduled pipeline (fetch, one model
   call per item, store, show) over an agent loop; use a loop only when the guide's criteria
   say the steps cannot be known in advance. Only fetch sources the site allows.
4. **Write the app.** Code plus `nerdit.toml` exactly as the guide shows. Keep a table of runs
   (start, end, items, result) and a page that lists it, so the user can see what happened.
5. **Commit, push, deploy.** Ask before pushing, then follow the `deploy` skill.
6. **Stop at every human step.** A missing model alias or provider key, an undefined secret or a
   spend limit is the user's to set: give them the error's `action_url` when there is one, or
   the console page the guide names, or `nerdit vars set <project> --secret --prompt KEY` to
   run themselves. Never ask for a secret value in chat. Then retry.
7. **Prove it runs.** For a scheduled job, trigger one run with the tool the guide names and
   read its result from the run list (runs are asynchronous). Show the user the outcome and the
   app's private URL; then offer `share-app` if others should open it.
