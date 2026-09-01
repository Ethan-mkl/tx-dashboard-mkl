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
assigned by Ethan — not just work on this dashboard file:

- **master-orchestrator** — entry point for open-ended or multi-part admin
  requests. Triages and delegates. Also the mandatory review gate for both
  specialists: neither ops-agent nor finance-agent's output reaches Ethan
  without passing through master-orchestrator's review first.
- **ops-agent** — runs all ops-related admin tasks Ethan assigns (delivery
  status, scope creep, renewal/delivery risk, team performance, general ops
  chores), executing where needed. Never reports completed work straight to
  Ethan without master-orchestrator reviewing it first.
- **finance-agent** — invoice tracking from email, finance-sheet updates,
  contract value/revenue exposure, renewal forecasting. Same review-gate
  rule as ops-agent — even "routine" invoice/timeline highlights go through
  master-orchestrator before reaching Ethan.

For a broad request ("give me the weekly admin rundown", "what needs
attention"), invoke `master-orchestrator`. For a request that's clearly and
entirely one domain, it's faster to call `ops-agent` or `finance-agent`
directly — though both still loop master-orchestrator in for review before
calling anything done. Neither specialist covers HR/talent data
(self-reviews, surveys, exit feedback) — that's intentionally out of scope
for now.

### Connector status (check before relying on finance-agent's email/sheet work)

- **Google Drive** — connected. Can search/read/download existing files
  (including Google Sheets content) and create new files. Cannot edit an
  existing sheet's cells in place.
- **Google Calendar** — connected at the org level, not yet enabled for chat.
- **Email (Gmail or similar)** — not connected at all. Until it is,
  finance-agent cannot scan inboxes for invoices or surface email reminders,
  and will say so rather than fabricating results.

Connect these via claude.ai → Settings → Connectors. Once done, finance-agent
picks them up automatically — no changes needed here unless the available
tool names differ from what's listed in `finance-agent.md`'s frontmatter.
