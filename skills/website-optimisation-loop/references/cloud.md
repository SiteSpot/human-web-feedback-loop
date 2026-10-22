# Running the loop on AB Split Test Cloud

AB Split Test Cloud is the WordPress-free edition: one script tag on the site, and a hosted service that serves the test engine, collects events, computes verdicts with the same statistics as the plugin, and exposes everything over MCP. There is no dashboard; the agent is the interface. It suits static sites, Next.js, Astro, Webflow, Framer, Shopify themes, plain HTML, and projects run mainly by an agent. It can run alongside the WordPress plugin.

**The tool descriptions your MCP client shows are the source of truth** for parameters; this page explains when to use what.

## Connecting

- Endpoint: `https://cloud.absplittest.com/mcp`, Streamable HTTP, header `Authorization: Bearer abst_…`.
- Claude Code:
  ```bash
  claude mcp add --transport http abst https://cloud.absplittest.com/mcp --header "Authorization: Bearer abst_YOUR_KEY"
  ```
- Keys are issued from the waitlist at <https://absplittest.com/cloud/>. Accounts start with a one-time allowance of 100,000 page views shared across their sites; `get_usage` reports where an account stands.

## Setting up a site

1. `create_site` with the site's domain returns the snippet. `get_snippet` returns it again later.
2. Put the snippet in the `<head>` of every page, usually the site's main layout or template.
3. Optionally turn on journeys with `update_site` (settings `{"journeys": true}`) to collect heatmaps, page analytics and session replays.

## The loop, mapped

| Step | What to use |
|---|---|
| SEE | `list_tests`, `get_test`; `list_heatmap_pages`, `get_heatmap_data`, `get_page_analytics`, `list_sessions`, `get_session` (journeys on). `get_heatmap_link` and `get_replay_link` give a person a 24-hour view. |
| FIND | The `write_test_brief` prompt plans a test from the site's history, audiences and thresholds; the `abst://tests/{id}/results.csv` resource has daily results |
| BUILD | `create_test` (draft unless `start: true`), `update_test`, audiences; `get_editor_link` lets a person build a point-and-click variation on their own page |
| PROVE | `get_results` (verdict with probability, expected loss, blocking reasons and likely duration; breakdowns by device or audience); `set_test_status` (`running`, `paused`, `complete`) |
| KEEP | `set_winner`, or a winner mode; for auto-optimise chains, `propose_next_round` then `create_next_round` |

## Things that trip agents up

- **`create_test` creates a draft by default.** Pass `start: true` only within your autonomy level.
- **Every test needs a primary conversion**; extra goals are reported per variation.
- **Test types:** `magic` (element changes by selector), `full_page` (redirect to a variant URL), `css` (a body class per variation), `onpage` (variations in your own markup).
- **Winner modes** (`mode`): `report` records verdicts only (default); `winner` shows the winner to everyone and completes the test; `thompson` shifts traffic toward the likely best; `autooptimize` (magic tests) completes on a winner and then waits for the agent to write the next round. **`winner` and `autooptimize` deploy automatically**, so use them only within your autonomy level.
- **Verdicts are computed every 15 minutes, not on read.** `recompute` forces one.
- **Audiences don't limit who's in a test.** They add a results breakdown with its own verdict.
- **Site-level decision thresholds** (minimum visits and conversions, probability, expected-loss threshold) are site settings. Read them; don't assume.

## Auto-optimise

A magic test in `autooptimize` mode completes itself on a winner. It then appears under `awaiting_next_round` in `list_tests`. `propose_next_round` gives the result, the history of earlier rounds and the copy to rewrite, and `create_next_round` builds the next round with the winner as its control. Add `reasoning` so later rounds can see why. The chain stops, with a reason, when the original wins, there's no decision, the winner changes no copy, or the round limit is reached.

This is the loop running with the agent writing each round. It deploys winners, so it belongs at autonomy level 3, or level 2 where the owner granted that category.
