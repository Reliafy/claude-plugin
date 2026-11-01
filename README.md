# Reliafy for Claude

Reliability engineering in Claude, on your own [Reliafy](https://reliafy.com) workspace: fit life
distributions (Weibull and the rest) to failure and suspension data, build and analyse reliability block
diagrams, work out replacement, proof-test and overhaul intervals, plan reliability tests, and forecast
fleet failures and spares. The calculations run on Reliafy, built on the open-source
[SurPyval](https://github.com/derrynknife/SurPyval) and
[RePyability](https://github.com/derrynknife/RePyability) libraries, and every result you save opens in
the Reliafy app.

The plugin has two parts:

- **The Reliafy MCP server** (`https://reliafy.com/mcp`), with six tools: `get` and `list` read your saved
  items, `analyse` runs every calculation (it saves nothing), `change` makes every write, `guide` has the
  how-tos, and `report_problem` tells us when something got in your way.
- **Skills** that teach Claude how to use them well: the censoring, unit and parameter conventions, how
  to model redundancy in an RBD, which action answers which question, and how to read the results. Each
  skill loads only when its topic comes up.

## Install

In Claude Code:

```
/plugin marketplace add Reliafy/claude-plugin
/plugin install reliafy@reliafy
```

Then run `/mcp`, pick **reliafy** and sign in with your Reliafy account in the browser. Reliafy is its own
OAuth server: the plugin holds no keys or secrets, and you can revoke access at any time under
Settings in the app.

## What you need

Using Reliafy from AI agents is part of **Reliafy Pro**. A Free account gets a few tool calls a month to
try it, with everything except fitting, fleets and simulations. Ask Claude "what's left on my Reliafy
plan?" to see where you stand. See [reliafy.com](https://reliafy.com) for the plans.

## Try it

- "Fit a Weibull to these bearing lives: 120, 340, 510 (still running), 700, 980 hours."
- "Build an RBD of two redundant pumps feeding one filter, then tell me its reliability at 8,760 hours."
- "What's the cost-optimal replacement interval for that model if a planned swap costs $500 and a
  failure costs $5,000?"
- "How many units do I need to test for 1,000 hours to show 90% reliability at 90% confidence?"
- "Share the model as a read-only link."

## The skills

| Skill | Covers |
|---|---|
| `reliafy` | The tools and the conventions every call follows (always relevant) |
| `life-data` | Fitting, evaluating and checking life data; per-demand; comparing groups; condition and life |
| `accelerated-tests` | Accelerated life tests: fit and extrapolate |
| `test-planning` | Demonstration, sequential, life, accelerated and comparison test plans |
| `repairable-systems` | Recurrent-event models, growth, next failure, overhaul |
| `rbd-building` | Building and editing reliability block diagrams |
| `rbd-analysis` | Reliability, availability, what to improve, intervals, fault trees, exports |
| `maintenance-strategy` | Replacement, shut-calendar, proof-test and overhaul intervals |
| `fleets` | Fleet failure forecasts, spares and alerts |
| `uploads` | Sending files in and importing them |
| `sharing` | Read-only share links and reports |
| `workspace` | Reading, tidying and deleting items; what changed |
| `plans` | Free and Pro, limits, the upgrade link |
| `failure-data-library` | Published, cited generic failure data |

The same how-tos are on the server (`guide(topic=…)`), so clients without skills, such as the claude.ai
connector, get them too. `guide(action='analyse.fit')` gives any action's full argument schema.

## Documentation, privacy and support

- API and tool documentation: <https://reliafy.com/api-docs>
- Privacy policy: <https://reliafy.com/privacy>
- Support: <hello@reliafy.com>
- Bugs in this plugin: [open an issue](https://github.com/Reliafy/claude-plugin/issues)

## Licence

[AGPL-3.0](LICENSE), the same as the Reliafy app.
