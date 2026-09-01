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
- **finance-agent** — invoice tracking from the "Finance Agent Inbox" Drive
  folder, finance-sheet updates, contract value/revenue exposure, renewal
  forecasting. Same review-gate rule as ops-agent — even "routine"
  invoice/timeline highlights go through master-orchestrator before
  reaching Ethan.

For a broad request ("give me the weekly admin rundown", "what needs
attention"), invoke `master-orchestrator`. For a request that's clearly and
entirely one domain, it's faster to call `ops-agent` or `finance-agent`
directly — though both still loop master-orchestrator in for review before
calling anything done. Neither specialist covers HR/talent data
(self-reviews, surveys, exit feedback) — that's intentionally out of scope
for now.

### Connector status (check before relying on finance-agent's Drive/sheet work)

- **Google Drive** — connected. Can search/read/download existing files
  (including Google Sheets content) and create new files. Cannot edit an
  existing sheet's cells in place.
- **Google Calendar** — connected at the org level, not yet enabled for chat.
- **Email (Gmail or similar)** — permanently unavailable, by org policy (not
  a pending setup step — Ethan's org does not allow connecting Gmail).
  Invoice intake runs instead through a dedicated Drive folder, **"Finance
  Agent Inbox"** (`1Hlo_ey4e-fEzsehqSBsXxmFidq6FhWFD`,
  https://drive.google.com/drive/folders/1Hlo_ey4e-fEzsehqSBsXxmFidq6FhWFD):
  Ethan drops invoice files there and finance-agent checks it via the Drive
  tools it already has. No email connector will ever be added for this
  workspace — don't suggest connecting one.

Sheet write-access is the one remaining real gap — connect a Sheets-capable
tool via claude.ai → Settings → Connectors if that becomes possible. Once
done, finance-agent picks it up automatically — no changes needed here
unless the available tool names differ from what's listed in
`finance-agent.md`'s frontmatter.
