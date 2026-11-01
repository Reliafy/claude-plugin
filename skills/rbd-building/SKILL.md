---
name: rbd-building
description: "Build and edit reliability block diagrams (RBDs) in Reliafy: structure rules for series, parallel, k-of-n and standby redundancy, block models, repairable diagrams, edits as op batches, outage logs. Use before creating or changing any RBD."
---

# Building RBDs in Reliafy

- Structure is everything: pure series only for items that must ALL work. Redundancy is modelled as redundancy: items named A/B, 1/2, duplex, dual, twin, standby, spare, backup, N+1 or "2 of 3" go in parallel (a standby node when the spare sits idle; a knode for k-of-n). Never wire "Pump A" -> "Pump B" in series. State your structural assumptions; if unclear, ask one question first. Check structure_summary, the diagram in one line, against what the user described.
- Never invent failure data: ask for MTBFs (or saved models) per class of block. If the user wants a starting point, give different values per class, set placeholder=true on every guessed model and tell the user.
- change rbd_create, graph form: nodes + edges, exactly one {id: input, type: input} and one {id: output, type: output}; two parallel blocks fan out from the node before and join at the node after; "2 of 3": three blocks into one knode (n=2). Stages form: stages in series, each with components in parallel (k_of_n voting).
- Block models: inline distribution_id + params, a mean (MTBF/MTTR) with its shape, a mixture, or a saved model (saved_model_id; same time unit).
- Repairable diagrams (repairable=true, analysed for availability): every block needs a repair model. Blocks may carry costs, preventive (age / block / condition replacement), inspection (hidden failures found by proof tests: offset staggers, coverage + full_test for partial tests), maintenance_group, crew_priority, repair_quality (Kijima: simulated figures); the diagram repair_crews, safety_function + target_sil (needs proof tests), costs (downtime_rate, horizon, discount_rate) and production (capacity demand). Common cause: common_cause_beta on a stage, or edit add_ccf (mgl for 3+ members).
- Non-repairable extras: phases (a phased mission), network (an undirected two-terminal network), covariate_schedules on a block with a saved regression model.
- Condition and life block (non-repairable diagrams only, one component block per unit): model = {saved_model_id: <joint model id>, joint_unit: {id: <running unit id>}} or joint_unit {age, readings: [[age, value], ...], covariates}. The block runs from its unit's age now: at mission time t its reliability is the unit's survival to age + t given its readings. Don't give it an age in current_state (it is already as of now). Repairable diagrams, exports to RePyability JSON and Download as Python refuse it (it lives in Reliafy).
- Edit, don't rebuild: change rbd_edit with one batch of ops (add_node / remove_node / update_node / add_edge / remove_edge / set / add_ccf / remove_ccf), all or nothing; dry_run checks first; if_updated_at refuses if the diagram changed. Samples are read-only: change rbd_clone first. get kind rbd reads a diagram's full graph.
- change rbd_upload_outage_log attaches a real outage log (asset, start, end; blank end = still down) to the user's diagram; analyse rbd_system_history reads it back.

## The actions

Every call is `tool(action=…, …)` (`kind=…` for get and list). `?` marks an optional argument; `guide(action='<tool>.<action>')` returns an action's full argument schema.

- `change` action=`rbd_create` (name, unit?, repairable?, nodes?, edges?, stages?, include_graph?, repair_crews?, maintenance_groups?, safety_function?, target_sil?, costs?, production?, phases?, network?): Build, validate and save a diagram: the graph form (nodes + edges, one input and one output node) or the simple stages form (stages in series, components in parallel within each).
- `change` action=`rbd_clone` (rbd_id, name?): Copy a diagram (a shared sample or the user's own) to a new one in the user's workspace, to edit (samples are read-only).
- `change` action=`rbd_edit` (rbd_id, ops, if_updated_at?, dry_run?, include_graph?): Change a saved diagram with a batch of ops, all or nothing (a failed op is named: ops[i]).
- `change` action=`rbd_delete` (rbd_id): Permanently delete one of the user's own diagrams (with any saved result).
- `change` action=`rbd_upload_outage_log` (rbd_id, csv?, columns?, unit?, window_start?, window_end?, date_order?, asset_map?, name?, upload_id?, sheet?): Save a real outage log (asset, start, end; blank end = still down) against one of the user's diagrams, as CSV text or an uploaded file, and return the system's observed history from it (as analyse rbd_system_history).
- `get` kind=`rbd` (id): A diagram's structure in the compact node form change rbd_create takes (nodes with their models, edges, unit), with the blocks still on placeholder models.
- `list` kind=`rbds` (): Saved reliability block diagrams: id, name, repairable or not, unit and block count.
