---
name: fleets
description: "Fleet failure forecasts, spares and email alerts in Reliafy (Pro): expected failures over a horizon with P10-P90 ranges, spares holdings, alert rules. Use when the user asks how many failures or spares a fleet will need."
---

# Fleets in Reliafy

- A fleet is in-service items run against one saved model; the app creates them and usage arrives through the ingest API. list kind fleets lists them (model_kind life, regression, alt or recurrent).
- analyse fleet_forecast: expected failures over the horizon with a P10–P90 range, per period and per item (the top 20 unless include_items; item_probabilities adds each item's chance by period). Recurrent fleets count every repeat failure.
- Spares: fleet_forecast with spares = {lead_time (in the fleet's periods), service_level (default 0.95), review, per_failure, stock, unit_cost} (or the fleet's saved spares settings) adds spares: hold (the stock for that chance of no stock-out over the lead time plus review), reorder_at, risk_at_stock, hold_cost, the same per period, and risk_curve ([stock, chance of running out]). repairable with turnaround: the pool covers the repair turnaround (one site). Read off the forecast's own distribution, not a normal approximation.
- Alerts: list kind fleet_alerts reads a fleet's rules with their current values; change fleet_create_alert adds one (above a threshold, a percent change, or x failures within y periods), checked whenever usage arrives, emailing the user once per crossing; at most 10 per fleet, own fleets only.

## The actions

Every call is `tool(action=…, …)` (`kind=…` for get and list). `?` marks an optional argument; `guide(action='<tool>.<action>')` returns an action's full argument schema.

- `list` kind=`fleets` (): Fleet forecasts: in-service items run against one saved model (model_kind life, regression, alt or recurrent).
- `analyse` action=`fleet_forecast` (fleet_id, include_items?, item_probabilities?, spares?): A fleet's live failure forecast: expected failures over the horizon with a P10-P90 range, per period and per item (the top 20 unless include_items; item_probabilities adds each item's chance by period); regression and ALT fleets at each item's covariates or stress; recurrent fleets count every repeat failure.
- `list` kind=`fleet_alerts` (fleet_id): A fleet's email alert rules, each with its current value, checked whenever usage arrives through the ingest API.
- `change` action=`fleet_create_alert` (fleet_id, kind, threshold?, percent?, x?, y_periods?): Save an email alert on one of the user's fleets: kind above (expected failures over the horizon cross threshold), change (they move by percent) or within (x failures now expected within y_periods).
