# tx-dashboard-mkl

A single static HTML file, `tx dashboard for github.html` — the TX Ops & HR
Dashboard. No build step, no backend, no dependencies beyond a CDN icon font;
open it directly in a browser. Its data lives inline in `<script>` as two
arrays:

- `contracts` — one row per client engagement: `client`, `team`, `value`,
  `status` (Active / At risk / On hold), `scope` (In-scope / Scope-creep),
  `risk` (low/medium/high), `renewal` (date).
- `teams` — one row per team: `id`, `name`, `score`, `trend`, `wins`,
  `blocks`.

The page has three tabs: Commercials & Ops, Team Performance, Talent
Insights (self-reviews / pulse surveys / exit feedback).

## Admin agent team

`.claude/agents/` defines a small delegation hierarchy for running admin work
against this project:

- **master-orchestrator** — entry point for open-ended or multi-part admin
  requests. Triages and delegates, doesn't do the work itself.
- **ops-agent** — delivery status, scope creep, renewal/delivery risk, team
  performance.
- **finance-agent** — contract value, revenue exposure, renewal forecasting,
  billing-style admin.

For a broad request ("give me the weekly admin rundown", "what needs
attention"), invoke `master-orchestrator`. For a request that's clearly and
entirely one domain, it's faster to call `ops-agent` or `finance-agent`
directly. Neither specialist covers HR/talent data (self-reviews, surveys,
exit feedback) — that's intentionally out of scope for now.
