---
name: failure-data-library
description: "Published, cited generic failure rates in Reliafy, for components with no data of the user's own."
---

# Reliafy's failure data library

The reliability data library (list kind library, get kind library_record, list kind library_sources): published generic failure data. Each record is one component and failure mode from one source, with its citation and licence.
- list kind library finds records by words (component, failure mode, system: "motor-driven pump fails to start"), filtered by source, equipment_type and record_kind (rate, per_demand, life); 25 a page, best first, with counts. get kind library_record gives a record in full, with block_model: the model a block would use.
- Read the units. A rate is per hour: per hour of running (fails to run) or per calendar hour (leaks, spurious operation, running or not). per_demand is a probability on each demand (a start, an opening), not a rate.
- Quote the mean with its 5th to 95th percentile range (the source's spread: gamma for a rate, beta for a demand), and the evidence (failures, demands or hours).
- In an RBD block: a rate is an exponential (failure_rate = the mean, x 24 per day, x 8,760 per year). A per-demand probability p is a cold-standby spare's startProb (1 - p), or a proof-tested item's failure on test. Converting a calendar rate to hours of running needs the hours a year it runs: ask the user, never guess.
- Handbook records (basis prediction_model, MIL-HDBK-217F: electronic parts) are a base failure rate per part type, not counted failures, and never mixed with field data. The rate depends on where the part works: get kind library_record with environment (GB ground, benign; GF ground, fixed; GM ground, mobile; NS naval, sheltered; AIC airborne, inhabited, cargo; and so on: the record's environments list each with its rate) gives the block model, base rate x the environment factor. Ask the user which environment; never guess. Say plainly that the handbook's other factors (temperature, stress, quality: prediction.not_applied) aren't applied and can move the rate a lot.
- These are generic estimates (NRC/INL: U.S. nuclear power plants; MIL-HDBK-217F: a 1995 prediction handbook), never the user's own data: say so, cite the record (licence.attribution), put the source in the block's label and suggest replacing it with their own data.
- Library lookups are never counted against the Free plan's tool calls.


## The actions

Every call is `tool(action=…, args={…})` (`kind=…` for get and list), the action's arguments in `args`. `?` marks an optional argument; `guide(action='<tool>.<action>')` returns an action's full argument schema.

- `list(kind='library', args={query?, source?, equipment_type?, record_kind?, page?=1, page_size?=25})`: Rates and per-demand probabilities by component and failure mode.
- `get(kind='library_record', args={id, environment?})`: One record in full: citation, licence, the block model to use.
- `list(kind='library_sources', args={})`: The sources, their licences and contents.
