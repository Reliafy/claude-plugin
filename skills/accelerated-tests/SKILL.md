---
name: accelerated-tests
description: "Accelerated life tests in Reliafy: fit a life-stress model (Arrhenius, Eyring, inverse power) to failures at several stress levels and extrapolate to use conditions with bounds. Use when data come from tests at raised temperature, voltage or load."
---

# Accelerated life tests in Reliafy

- change model_fit_alt fits and saves failure times at several stress levels with a life-stress relationship (life_model: arrhenius, eyring, inverse_power, dual models…) and a distribution: inline data + stresses (one row per unit) or a dataset (time_column + stress_columns). Same censoring flags as life data, inspection data (flag 2 with data_right) and delayed entry (trunc_left) included. Thermal models need kelvin. Fitting is Pro.
- analyse alt_use_level extrapolates it to the use stress: characteristic and mean life, B-lives, mission reliability, the acceleration factor against a test stress, with bounds: wald (default), lr, or bootstrap (Pro, runs as a job: get kind job until done; not for interval or left-censored data). Quote the bounds with the values: an ALT answer is an extrapolation, and with few failures (a warning says so) no bound holds its stated confidence.

## The actions

Every call is `tool(action=…, …)` (`kind=…` for get and list). `?` marks an optional argument; `guide(action='<tool>.<action>')` returns an action's full argument schema.

- `change` action=`model_fit_alt` (name, life_model?, distribution?, data?, data_right?, stresses?, censored?, counts?, trunc_left?, trunc_right?, stress_names?, c_invert?, dataset_id?, time_column?, time_right_column?, stress_columns?, censor_column?, count_column?, trunc_left_column?, trunc_right_column?, unit?, stress_units?): Fit and save an accelerated life test model: failure times at several stress levels and a life-stress relationship, inline (data + stresses) or from a dataset (time_column + stress_columns), with the usual censoring flags (inspection data: flag 2 with data_right) and delayed entry.
- `analyse` action=`alt_use_level` (model_id, use_stress, ref_stress?, mission_time?, confidence?, bounds_method?, wait_seconds?): Extrapolate a saved ALT model to its use stress: characteristic, mean and B-lives, the mission reliability, the acceleration factor against ref_stress, with bounds (wald default, lr, or bootstrap: Pro, runs as a job).
- `analyse` action=`plan_accelerated_test` (life_params, shape, use_stress, censor_time, life_model?, distribution?, high_stress?, units?, levels?, stresses?, units_at?, b_life?, confidence?, unit?): An accelerated life test plan from planning values: stresses, units at each, expected failures and the B-life's precision at the use stress (or the precision of your own test).
