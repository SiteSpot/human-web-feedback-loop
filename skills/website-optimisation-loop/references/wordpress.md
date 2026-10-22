# Running the loop on AB Split Test for WordPress

AB Split Test runs inside the WordPress install. Visits, conversions, heatmaps and learnings are stored in the site's own database, and nothing is metered. Agents use it over MCP through the WordPress Abilities API. **The ability descriptions your MCP client shows are the source of truth** for parameters; this page explains when to use what.

## Connecting

The site needs AB Split Test and the WordPress MCP Adapter. Depending on the adapter version, abilities appear as tools directly or are listed and run through the adapter's discover-abilities and execute-ability tools; the names below are the ability names either way. The MCP client authenticates with a WordPress Application Password for a user who can manage tests. The plugin's settings screen shows the connection details for each client; the public docs are at <https://absplittest.com/api-mcp-cli/>. WP-CLI (`wp absplittest …`) and a REST API are also available.

## Editions

What's available depends on the edition and plan. Don't promise a feature; if a tool or option isn't offered, it isn't available on this site.

- **AB Split Test Lite** (free, wordpress.org) exposes the core test abilities: list, create, details, results, update settings, update status. Each test compares the original with one variation. Goals are page visits and element clicks. Heatmaps exist in the admin but aren't exposed to agents.
- **AB Split Test (paid)** adds the rest: multiple variations, more goal types including revenue, audiences, heatmap and page-analytics abilities, site content read and write, goal management, and the automatic winner modes below.

## The loop, mapped

| Step | What to use |
|---|---|
| SEE | `list-tests` and `get-test-results` on completed tests; heatmap pages, heatmap data and page analytics abilities (paid); site content and builder context to understand the page |
| FIND | Completed tests' results and descriptions; the plugin's Optimization Hub records learnings for people, but they aren't yet exposed as an ability |
| BUILD | `create-test` with status `idea` or `draft`; goals and audiences abilities (paid); `update-test-settings` |
| PROVE | `get-test-results`; `update-test-status` |
| KEEP | `update-test-status` → `complete`; the plugin records the learning; make a winner permanent through the page content, or through the winner mode if the owner allows automatic deployment |

## Things that trip agents up

- **Statuses:** `idea` (a hypothesis only), `draft` (not running, also used to pause), `pending`, `publish` (running), `complete`. There is no `active` or `paused`.
- **The create-test tool asks you to confirm the conversion goal with the user** before creating a runnable test. Do.
- **Magic (point-and-click) tests:** the first entry in each element's variations is the original. Find reliable selectors by opening the page with `?abhash=1`, which lists each visible element's selector at the bottom. The same parameter previews variations.
- **Test types:** `magic` (element changes), `full_page` (original page vs a variant page), `ab_test` (sections tagged in a page builder or the block editor), `css_test` (code-level).
- **Winner modes** (`mode`): `notify` shows the winner to every visitor while the test keeps running; `autocomplete` shows the winner and completes the test; `thompson` is a bandit that never declares a winner; `autooptimize` (magic tests) completes on a winner and builds the next round with AI. **Every mode except a plain report deploys something automatically**, so pick one only within your autonomy level.
- **Caching:** the plugin purges the common caches itself. If visits aren't recording, read `get-settings` and tell the user; don't change cache configuration.
- **Results** include visits, conversions, rates, likelihood of winning, uplift, time remaining and a shareable report URL. Report the platform's verdict; don't recompute it.
