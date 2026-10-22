# Recommended MCP and product changes

Changes to AB Split Test (WordPress plugin and Cloud) that would make the two skills more reliable. Ordered by how much they reduce the chance of an agent doing the wrong thing. Each notes what exists today, checked against the code on 21–22 October 2026.

## 1. A structured decision block in results, in both editions

**Today.** Cloud's `get_results` returns a verdict block with `best`, `probability`, `winner`, `blocked_by`, `expected_loss`, `min_conversions` and `likelyDuration`. The plugin computes the same `blocked_by` internally (`abst_stats_blocked_by`), but the WordPress `get-test-results` ability describes its output loosely ("likelihood of winning, … winner confidence").

**Change.** Return the same top-level block from both, for example:

```json
"decision": {
  "status": "collecting | eligible | winner | no_decision | invalid",
  "best": "variation key",
  "probability_best": 0.978,
  "expected_loss": 0.0004,
  "thresholds": { "probability": 0.95, "expected_loss": 0.001, "min_visits": 50, "min_conversions_per_variation": 25, "min_runtime_days": 14 },
  "blocked_by": ["runtime"],
  "validity": { "srm": "ok", "tracking": "ok" },
  "projected_days_remaining": 6
}
```

**Why.** The skills tell agents to defer to the platform. That only works if the platform states its decision explicitly, names its thresholds, and uses the term "probability to be best" consistently instead of "confidence".

## 2. Sample-ratio mismatch check

**Today.** No SRM check in either edition's statistics.

**Change.** Run a chi-square test of observed visits per variation against the configured weights (p < 0.01) and report it in `validity`. Block a winner while it fails. For Thompson tests, compare within allocation periods or skip with a note.

**Why.** It's the most common reason a confident result is wrong (redirects, caching, script errors, bots dropping one arm). Agents can't reliably compute it from summary tables.

## 3. Minimum runtime as a test setting

**Today.** Neither edition has a minimum runtime; a verdict can arrive on day one if the thresholds are met, and the automatic winner modes will act on it.

**Change.** A per-test or per-site `min_runtime_days` (default 7, recommended 14), reported in `blocked_by` while unmet.

**Why.** The skills tell agents to wait for a full weekly cycle, but `winner`, `autocomplete`, `notify` and `autooptimize` modes deploy on the verdict alone. The platform should enforce what the method requires.

## 4. A pre-launch traffic estimate

**Today.** `likelyDuration` (Cloud) and time remaining (plugin) exist once a test is running. The plugin already has `abst_stats_project_duration`.

**Change.** An `estimate_test` tool: page or URL, number of variations, primary goal, minimum detectable lift → expected days to a decision, based on the page's recent visits and conversions.

**Why.** It turns the developer skill's "worth testing but too little traffic" judgement into a lookup, and stops agents launching tests that can't decide.

## 5. Guardrails as first-class test settings

**Today.** Secondary goals are reported per variation (Cloud allows ten), but nothing marks a goal as a guardrail or acts on it.

**Change.** Let a goal be flagged as a guardrail with a direction and action (`pause` or `block_winner`), and report guardrail status in the decision block.

**Why.** The skills tell agents that guardrails can veto a winner. Today that relies on the agent remembering to look.

## 6. Learnings over MCP on WordPress

**Today.** The plugin records structured learnings on completion (`includes/learnings.php`) and shows them in the Optimization Hub, but there's no ability to read them. Cloud has the `write_test_brief` prompt and round reasoning on auto-optimise chains.

**Change.** A `list-learnings` ability (and the equivalent in Cloud), returning the same fields as the skill's learning record.

**Why.** SEE and FIND depend on not repeating disproved ideas. Agents can only do that if they can read what was learned.

## 7. Permission-scoped API keys

**Today.** WordPress uses an Application Password with the user's full capabilities. Cloud keys are account-wide.

**Change.** Key scopes: `read`, `draft` (create and edit drafts, no status changes to running, no winners), and `full`. Optionally, scopes for deployment-capable winner modes. Hide tools a key can't use.

**Why.** Autonomy level 1 should be enforced by the platform, not only by the agent's discipline. Convert and Statsig both do this, and there are public reports of AI clients launching tests unasked.

## 8. A capabilities tool

**Today.** What's available depends on edition and plan. Lite exposes six test abilities; the paid plugin many more. The absplittest.com pricing page and the Lite readme currently disagree on Lite's limits.

**Change.** A `get-capabilities` tool returning edition, plan, limits (tests, variations, goals, page views) and which winner modes and goal types are allowed.

**Why.** Agents shouldn't promise or attempt features the site doesn't have. Also: reconcile the pricing page with the Lite readme.

## 9. Consistent status names

**Today.** WordPress uses `idea`, `draft`, `pending`, `publish`, `complete`, with `draft` doubling as paused. Cloud uses `draft`, `running`, `paused`, `complete`.

**Change.** Accept `running` and `paused` as aliases in the WordPress abilities, and return a normalised status alongside the WordPress one.

**Why.** Agents moving between editions get this wrong. "Pause" mapping to `draft` is particularly easy to miss.

## 10. Explicit deploy semantics on winner modes

**Today.** The mode descriptions explain the behaviour, but not that most modes deploy automatically.

**Change.** Add to each mode's description whether it changes what all visitors see without further action, and return `deploys_automatically: true|false` in test details.

**Why.** The skills treat automatic deployment as a level 2 or 3 permission. The tool should make that obvious at the point of choice.
