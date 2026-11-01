---
name: test-planning
description: "Plan reliability tests with Reliafy: demonstration (success-run, MTBF), sequential, life-test precision, accelerated test plans and before/after comparisons (sample size, power). Use when the user asks how many units or how long to test."
---

# Test planning in Reliafy

Test planning: every plan is exact or numerical and saves nothing (analyse actions). Each answer leads with a plain summary: relay it, with the assumptions that matter.
- Show a target is met: plan_demonstration_test (fixed length; method attribute or mtbf; with a good design and producer_risk it keeps both risks). plan_sequential_test decides as failures come in, usually sooner: it needs the target and a good design (design_mtbf / design_reliability), and with failures_seen plus time_seen (or units_seen) says accept, reject or continue. Budget for its longest run (max_time / max_units).
- Estimate a life: plan_life_test from planning values (params, the shape matters most): the precision of a B-life or the mean for units on test (solve_for precision), the units for a precision (units, within_pct: 20 = ±20%) or how long to run (length). test_end time (censor_time), failures (failure_fraction) or all.
- Accelerated tests: plan_accelerated_test from planning values of the non-repairable model (life_params; a similar part's change model_fit_alt gives them) and shape: stresses, units at each and expected failures up to high_stress (levels 2 = optimum, 3 = compromise, which can show the model is wrong), or the precision of your own stresses and units_at. Relay warnings: stresses with under 3 expected failures make the precision optimistic.
- Did the change work? plan_comparison. kind logrank: old and new units side by side (hazard_ratio new/old, event_probability = chance a unit fails during the study): sample_size or power. kind rate_ratio: one fleet before and after (failures_before in exposure_before): exposure after needed for a rate_ratio, the smallest detectable drop for exposure_after, or power. A null exposure_after means no exposure is enough: say so.
- Checking a plan by simulation is in the app (Pro), not here.

## The actions

Every call is `tool(action=…, …)` (`kind=…` for get and list). `?` marks an optional argument; `guide(action='<tool>.<action>')` returns an action's full argument schema.

- `analyse` action=`plan_demonstration_test` (reliability?, confidence?, mission_time?, failures?, test_multiple?, shape?, units?, method?, mtbf?, design_reliability?, unit?, design_mtbf?, producer_risk?, b_life?, shape_model_id?, shape_bound?): Plan a reliability demonstration test: units, test time per unit and failures allowed to show reliability R over a mission at confidence C (success run / binomial; with a known Weibull shape, test longer with fewer units; or an MTBF test).
- `analyse` action=`plan_sequential_test` (method?, mtbf?, reliability?, design_mtbf?, design_reliability?, confidence?, producer_risk?, unit?, failures_seen?, time_seen?, units_seen?): A truncated sequential test of an MTBF or a reliability against a good design: accept/reject lines, both risks as run, average length against the fixed-length test; with failures_seen, accept, reject or continue.
- `analyse` action=`plan_life_test` (params, distribution?, solve_for?, units?, within_pct?, test_end?, censor_time?, failure_fraction?, quantity?, b_life?, confidence?, unit?): How precisely a life test estimates a B-life or the mean from planning values, or the units or test length for a precision.
- `analyse` action=`plan_accelerated_test` (life_params, shape, use_stress, censor_time, life_model?, distribution?, high_stress?, units?, levels?, stresses?, units_at?, b_life?, confidence?, unit?): An accelerated life test plan from planning values: stresses, units at each, expected failures and the B-life's precision at the use stress (or the precision of your own test).
- `analyse` action=`plan_comparison` (kind, solve_for, hazard_ratio?, event_probability?, units_per_group?, failures_before?, exposure_before?, exposure_after?, rate_ratio?, power?, confidence?, one_sided?, unit?): Did the change work?
