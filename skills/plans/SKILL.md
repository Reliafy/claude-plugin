---
name: plans
description: "Reliafy plans for AI agents (Free vs Pro): what to do when a call is refused for the plan or a limit."
---

# Reliafy plans and limits

- Using Reliafy from AI agents is part of Reliafy Pro: every tool, no limit. Free gets a few tool calls a month to try it (every tool except fitting and fleets, within the Free storage limits; availability simulation and some design searches are Pro too). A retired Agent plan keeps a daily quota until it ends.
- get kind account: the plan, the calls used and left (and when they reset), storage against the limits, whether simulation is included. It, guide, get kind upgrade_link, change upload_send_part and library lookups are never counted.
- When a tool says a limit or the plan stops it, tell the user plainly what it says — the limit, when it resets, how to upgrade — and don't retry. Calls refused that way aren't counted.
- Without Pro, fit locally with SurPyval (pip install surpyval; 0 = failed, 1 = still running) and save the parameters with change model_save; run a diagram's simulation locally with analyse rbd_export_python.
- get kind upgrade_link gives the user a Stripe payment link to open themselves; nothing is charged until they complete it. Never say an upgrade has happened.

## The actions

Every call is `tool(action=…, args={…})` (`kind=…` for get and list), the action's arguments in `args`. `?` marks an optional argument; `guide(action='<tool>.<action>')` returns an action's full argument schema.

- `get(kind='account', args={})`: The plan and what's left: on Free the tool calls used and left this month and when they reset (Pro: unlimited); storage per kind against the plan's limits; whether simulation is included; how to upgrade.
- `get(kind='upgrade_link', args={plan?=pro})`: A link the user opens to subscribe to Reliafy Pro on Stripe's checkout page.
