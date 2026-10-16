---
name: custom-domain
description: Serve a Nerdit app at the user's own subdomain (for example app.example.com) and walk them through the one DNS record it needs until HTTPS is active. Use when the user wants a custom domain, their own URL, or to point a domain at a Nerdit app.
---

# Add a custom domain to a Nerdit app

Needs the Pro plan; at most 3 domains per account.

1. **Find the app.** `nerdit_list_nodes` for the `node_id`, and the app's name on that machine
   (`service_name`, from `nerdit_get_project` or the deploy). Ask for the exact host.
   A bare domain such as `example.com` is refused: suggest `app.example.com` or `www.example.com`,
   with a redirect from the bare name at their registrar.
2. **Add it.** `nerdit_add_domain` with `{node_id, service_name, host}`.
3. **Give the record, exactly.** Tell the user to create `CNAME <record.name> -> <record.target>`
   at their DNS provider. On Cloudflare it must be **DNS only** (grey cloud), not proxied.
   Nothing is served until the record is seen, and a name not verified within 24 hours
   (`expires_at`) is removed.
4. **Check.** Once they say it is done, `nerdit_check_domain` with the same arguments.
   `awaiting_dns`: the record is not visible yet; check the name and target and try again in a
   few minutes. `pending`: the certificate is being issued; check again shortly. `active`: give
   them `https://<host>`. `error`: the record or certificate went away; check the record.

To list or remove: `nerdit_list_domains`, `nerdit_remove_domain` (ask first; their DNS record
stays and they may delete it).
