---
name: maintenance-strategy
description: "When to replace, test or overhaul with Reliafy: age replacement, shut calendars, proof-test and overhaul intervals."
---

# Maintenance strategy in Reliafy

- analyse optimal_replacement: the cost-optimal age-replacement interval from a non-repairable model (model_id, or distribution_id + params) and the planned and unplanned costs. beneficial=false: run to failure is cheaper.
- analyse failure_finding_interval: the proof-test interval keeping a hidden protective function (relief valve, trip, standby unit) at a target availability.
- analyse optimal_overhaul: the overhaul interval of a repairable system from a minimal-repair recurrent model.
- analyse calendar_replacement: when replacing costs less at shuts (annual, quarterly, weekly stops) than between them, the long-run best rule: at each shut, replace the part if it is older than a threshold age (the threshold depends on when the next cheaper shut comes). Give the model in a time unit, the shuts (name, cost, every N days/weeks/months/years from a start date, or dates), failure_cost and anytime_cost (a planned replacement between shuts; omit when only shuts allow one). Today is time 0; months are 365.25/12 days. Back: cost_rate (per model unit) and cost_per_year, the rule in words, by_kind (replacements and cost a year per shut kind and failures), comparisons (run to failure, age replacement ignoring the shuts, every shut of a kind) and the next 12 months' shuts with each one's threshold and chance of a replacement. method dp (default) and integration (no anytime_cost) are exact and free; sampling (Pro) is for calendars whose cycle is very long, with cost_rate_se. Quote the unit.
- analyse next_replacement: the same calendar plus the age of the unit in service today: replace it now, at the next shut of each kind, between shuts, or run it to failure, each choice's cost, the best, and replace_from_age (from that age replacing now beats waiting).
- With a saved Weibull (or Crow-AMSAA) model fitted to data, shape_uncertainty gives the answer at each end of the shape's 95% interval: relay uncertainty_note when present — the recommendation is less firm than it reads (for a protective function, the shorter interval is the safe side).
- A whole diagram's intervals together: analyse rbd_optimise_intervals. A demonstration test or a B-life check: analyse plan_demonstration_test / check_life_requirement. Fleets: guide topic fleets.

## The actions

Every call is `tool(action=…, args={…})` (`kind=…` for get and list), the action's arguments in `args`. `?` marks an optional argument; `guide(action='<tool>.<action>')` returns an action's full argument schema.

- `analyse(action='optimal_replacement', args={planned_cost, unplanned_cost, model_id?, distribution_id?, params?, unit?, include_curve?})`: The cost-optimal age-replacement interval for a component with a known life model; beneficial=false means run to failure is cheaper.
- `analyse(action='calendar_replacement', args={shuts, failure_cost, unit?, model_id?, distribution_id?, params?, anytime_cost?, today?, max_age?, method?=dp, include_schedule?, wait_seconds?=20})`: The long-run best replacement rule over a calendar of shuts (today = time 0): the threshold age at each shut, the cost against running to failure and plain age replacement, and the next 12 months by date.
- `analyse(action='next_replacement', args={age, shuts, failure_cost, unit?, model_id?, distribution_id?, params?, anytime_cost?, now_cost?, today?, max_age?})`: For one unit in service on a shut calendar: replace it now, at the next shut, between shuts or run it to failure, each choice's cost and the age from which replacing now pays.
- `analyse(action='failure_finding_interval', args={target_availability, model_id?, distribution_id?, params?, unit?})`: The proof-test interval keeping a hidden protective function (relief valve, trip, standby unit) at a target availability.
- `analyse(action='optimal_overhaul', args={model_id, cost_repair, cost_overhaul, t_max?, include_curve?})`: The optimal overhaul interval of a repairable system (minimal repair between overhauls) from a saved minimal-repair recurrent model; only a deteriorating system (β > 1) has one.
