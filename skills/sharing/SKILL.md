---
name: sharing
description: "Share Reliafy items as read-only pages anyone with the URL can open, build multi-item reports, list and revoke links. Use only when the user asks to share or publish something."
---

# Sharing from Reliafy

- change share_link publishes a read-only page of one of the user's own items (model, dataset, rbd, degradation_model, strategy_analysis, fleet, recurrent_model) that anyone with the URL can open without an account. password=true: Reliafy generates a passphrase, returned once — tell the user to send the link and the passphrase to the recipient separately. expires_in_days for a link that ends; label says who it's for. Samples can't be shared (change rbd_clone one first).
- change share_report puts several items on one print-ready page with a title, an author line, an introduction and a note per item. Write the notes from the items' results, quoting their figures as the tools returned them.
- list kind share_links shows the live links; change share_revoke ends one at once.

## The actions

Every call is `tool(action=…, …)` (`kind=…` for get and list). `?` marks an optional argument; `guide(action='<tool>.<action>')` returns an action's full argument schema.

- `change` action=`share_link` (kind, id, password?, expires_in_days?, label?): Publish a read-only page of one of the user's own items that anyone with the URL can open, no account needed; each call makes a new link.
- `change` action=`share_report` (title, items, author?, introduction?, password?, expires_in_days?, label?): Several of the user's items on one print-ready share page: a title, author line, introduction and a note per item, each item as its share page shows it.
- `change` action=`share_revoke` (token): Revoke a share link (by token or URL): it stops working at once.
- `list` kind=`share_links` (item_kind?, item_id?): The user's live share links, newest first, optionally for one kind (item_kind) or item (item_id): URL, item, label, protected, expiry.
