---
name: repairable-systems
description: "Repairable systems in Reliafy: recurrent-event models (Crow-AMSAA, NHPP, imperfect repair), reliability growth, next failure, growth projection and overhaul intervals. Use when units fail, are repaired and fail again."
---

# Repairable systems in Reliafy

- change model_fit_recurrent fits (and saves, unless save=false) a recurrent-event model from event histories: inline times + systems (censored: 0 failure, 1 end of observation; or observed_to) or a dataset. Minimal repair: crow_amsaa (default), duane, hpp, cox_lewis; covariates on the repair rate: pi_nhpp; imperfect repair: grp_i / grp_ii, ara, ari, g1. windows give observation gaps; causes add each cause's growth. Fitting is Pro.
- get kind model on one: the growth verdict with β's 95% interval, ROCOF and MTBF with bounds, the demonstrated MTBF's lower bound, trend and goodness-of-fit tests, and (imperfect repair) the repair effectiveness and each system's next failure.
- analyse next_failure: at a system's age, the chance of a failure within given times and the time to the next one. analyse growth_projection: the MTBF a growth test reaches once its fixes are in (BD modes with fix-effectiveness factors).
- analyse optimal_overhaul: the overhaul interval for a deteriorating system (minimal-repair model, β > 1).

## The actions

Every call is `tool(action=…, …)` (`kind=…` for get and list). `?` marks an optional argument; `guide(action='<tool>.<action>')` returns an action's full argument schema.

- `change` action=`model_fit_recurrent` (model?, name?, save?, times?, systems?, censored?, observed_to?, causes?, covariates?, windows?, dataset_id?, system_column?, time_column?, censor_column?, observed_to_column?, cause_column?, covariate_columns?, window_start_column?, window_end_column?, baseline?, memory?, unit?): Fit a recurrent-event (repairable-system) model to failure histories, many failures per system, and save it (save=false: report only).
- `analyse` action=`next_failure` (model_id, age, within?, quantiles?): A repairable system's next failure from a saved recurrent model at its age: the failure intensity and MTBF now, the mean and quantiles of the time to the next failure, and the chance of a failure within each time ahead.
- `analyse` action=`growth_projection` (model_id?, fef?, bd_modes?, bc_modes?, test_end?, mode_column?, x?, modes?, systems?, unit?): Reliability growth projection (AMSAA-Crow, MIL-HDBK-189C): the MTBF a growth test reaches once its fixes are in.
- `analyse` action=`optimal_overhaul` (model_id, cost_repair, cost_overhaul, t_max?, include_curve?): The optimal overhaul interval of a repairable system (minimal repair between overhauls) from a saved minimal-repair recurrent model; only a deteriorating system (β > 1) has one.
