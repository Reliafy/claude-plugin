---
name: workspace
description: "Read, tidy and delete saved Reliafy items (models, datasets, RBDs), see what changed in the workspace, and report a problem with the tools. Use for listing, renaming or deleting items and for workspace summaries."
---

# The Reliafy workspace

- Reads: list kind models / datasets / rbds / fleets / share_links / rbd_runs, and get kind model / dataset (a page of rows at a time; part=measurements for a dataset's readings) / rbd / rbd_run / job / upload. Shared samples are listed (is_sample) and readable, never changed.
- A dataset is one document: its life data (its table) and, when added, its measurements (readings on the same units at ages since new; change dataset_add_measurements / dataset_remove_measurements). The list shows a measurements entry where there is one. Datasets never point at each other: readings for a dataset's units go on that dataset.
- list kind workspace_changes: what changed over a period (since a date or the last visit, or days), computed from what Reliafy stores: new and changed items, refits that moved B10 or MTTF, moved forecasts, fired alerts, moved simulations. Start from its sentence; report not_compared as unknown, never guess.
- Tidying: change model_update / change dataset_update rename or annotate (never the data or the fit).
- Deleting: change model_delete, change dataset_delete, change rbd_delete — permanent, own items only. Only on the user's explicit request: confirm by name first, and relay what the response lists as affected. A dataset a model was fitted to, or a model a fleet runs on, can't be deleted until those are dealt with.
- Every saved item has a url: give it to the user so they can open it in Reliafy.
- When a tool gets in your way (an error, a result that makes no sense, an action that's missing, a default that's wrong, a guide that's unclear), report_problem report it: kind, a one-line summary, the tool and action, what you were trying, what you expected and what happened. Tell the user you did. It never counts against a plan. report_problem status (with the report_id, or none for your latest) says later whether it was acknowledged or fixed, and in which release; when it's fixed, try again.

## The actions

Every call is `tool(action=…, …)` (`kind=…` for get and list). `?` marks an optional argument; `guide(action='<tool>.<action>')` returns an action's full argument schema.

- `list` kind=`models` (model_type?): Saved models: id, name, kind, distribution, fitted params, time unit (model_type: life or recurrent).
- `list` kind=`datasets` (): Saved datasets with their columns and row counts (and their measurements, where added).
- `list` kind=`rbds` (): Saved reliability block diagrams: id, name, repairable or not, unit and block count.
- `list` kind=`workspace_changes` (since?, days?): What changed in the user's workspace over a period (days, or since a date or the last visit), computed from what Reliafy stores, never estimated: new and changed items, refits whose B10 or MTTF moved, fleet forecasts that moved and alerts that fired (Pro), simulations that moved.
- `get` kind=`model` (id): A saved model in full, life or recurrent.
- `get` kind=`dataset` (id, offset?, limit?, part?): A dataset's name, notes, columns with types, row_count and a page of rows (offset, limit; part for its measurements); next_offset pages on.
- `get` kind=`rbd` (id): A diagram's structure in the compact node form change rbd_create takes (nodes with their models, edges, unit), with the blocks still on placeholder models.
- `change` action=`model_update` (model_id, name?, notes?): Rename one of the user's own models (life or recurrent) or set its notes; never the fit or data, never a sample.
- `change` action=`model_delete` (model_id): Permanently delete one of the user's own models (life, recurrent, ALT or degradation).
- `change` action=`dataset_update` (dataset_id, name?, notes?): Rename one of the user's own datasets or set its notes; never the data, never a sample.
- `change` action=`dataset_delete` (dataset_id): Permanently delete one of the user's own datasets.
- `change` action=`dataset_remove_measurements` (dataset_id): Take the measurements off a dataset (its life data stays); refused while a model reads them.
- `change` action=`rbd_delete` (rbd_id): Permanently delete one of the user's own diagrams (with any saved result).
- `report_problem` action=`report` (kind, summary, tool?, report_action?, trying?, expected?, happened?, detail?): File a report when a tool got in your way; returns its report_id.
- `report_problem` action=`status` (report_id?): A report you filed (or your latest): new, acknowledged, fixed (fixed_in: the release), wont_fix or duplicate.
