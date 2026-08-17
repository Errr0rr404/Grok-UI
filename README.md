<div align="center">

<img src="https://raw.githubusercontent.com/joeynyc/Grok-UI/main/docs/grok-ui-red-mark.png" width="128" alt="Grok UI red command mark"/>

# Grok UI

**Every Grok session — one living command center.**

An unofficial, local-first dashboard for people who already run
[Grok Build](https://github.com/xai-org/grok-build) on machines they operate.
Watch active agents, control sessions over ACP, inspect Git changes,
and move through local history without leaving the browser.

![Node.js 22+](https://img.shields.io/badge/Node.js-22%2B-5FA04E?logo=nodedotjs&logoColor=white)
![React 19](https://img.shields.io/badge/React-19-149ECA?logo=react&logoColor=white)
![Express 5](https://img.shields.io/badge/Express-5-000000?logo=express&logoColor=white)
![Vite 8](https://img.shields.io/badge/Vite-8-646CFF?logo=vite&logoColor=white)
![License: MIT](https://img.shields.io/badge/license-MIT-E44738)

<a href="https://raw.githubusercontent.com/joeynyc/Grok-UI/main/docs/grok-ui-dashboard.png">
  <img src="docs/grok-ui-dashboard.png" width="900" alt="Grok UI Event Horizon dashboard with all 12 product areas and Privacy Mode enabled"/>
</a>

[**Watch the 24-second product tour**](https://github.com/joeynyc/Grok-UI/releases/download/v0.5.1/grok-ui-dashboard-tour-final-white-logo.mp4)
· [**See what’s new in v0.11.0**](https://github.com/joeynyc/Grok-UI/releases/tag/v0.11.0)
· [**Quickstart**](#quickstart) · [**What it does**](#what-it-does)
· [**Architecture**](#architecture) · [**Security**](#privacy-and-security)

</div>

> [!NOTE]
> Grok UI is an independent community project. It is not affiliated with or endorsed by xAI.

## Who it is for

Grok UI is a single-user companion for developers who already use the Grok CLI
locally, or who operate a small set of their own machines. It is not a hosted
multi-tenant service and does not replace the CLI.

You need a working `grok` install and Grok Build account access. The dashboard
does not scrape the terminal UI or send Grok credentials to the browser.
Optional Fleet monitoring and remote sessions are for hosts you register
yourself.

## What it does

<table>
<tr>
<td width="50%" valign="top">

**◉ A runtime that tells the truth**

Active agents come from Grok’s real process registry. Turns, phases,
reasoning, responses, tools, context, and cost updates flow into the
dashboard over one live event stream.

</td>
<td width="50%" valign="top">

**⌘ Native session control**

Create, resume, prompt, approve, and cancel Grok sessions through the
Agent Client Protocol. Permission choices are rendered exactly as Grok
provides them—never guessed or auto-approved.

</td>
</tr>
<tr>
<td width="50%" valign="top">

**◇ One Session Console per agent**

Open any recorded CLI session or managed lane in a clearly labeled Session
Console to chat with the agent, review its activity, inspect changes, manage
permissions, send follow-ups, and launch a session-scoped web preview. Managed
sessions survive dashboard restarts.

</td>
<td width="50%" valign="top">

**↗ Git changes as they happen**

The selected repository is watched live. Staged, unstaged, and untracked
files update automatically, with bounded diffs and no manual refresh
required.

</td>
</tr>
<tr>
<td colspan="2" valign="top">

**⌁ Cross-session workflow command field**

Grok Build v0.2.112+ workflow updates become one live run field: phase
progression, per-agent token usage, models, duration, budget use, failures,
results, and safe Pause, Resume, or Stop controls. Searchable, paginated rosters
stay usable as a workflow scales. Failed runs expose recovery only when Grok
reports that the current process can resume them.

</td>
</tr>
<tr>
<td colspan="2" valign="top">

**◎ Secure remote sessions**

Register trusted Grok UI host agents over managed SSH forwarding or a private
Tailscale network. The Fleet view shows identity, versions, capabilities,
health, latency, last seen, freshness, and bounded telemetry. With a separate
per-host control grant, start a managed Grok session and continue the same live
conversation from another device, including follow-ups, Grok permission
choices, and interruption.

</td>
</tr>
</table>

## Architecture

<div align="center">
<a href="https://raw.githubusercontent.com/joeynyc/Grok-UI/main/docs/architecture.svg">
<img src="docs/architecture.svg" width="820" alt="Browser connected to a local Grok UI supervisor, local state and ACP control, plus authenticated read-only remote host agents"/>
</a>
</div>

Grok UI runs as one local Express supervisor. The browser receives runtime,
control, dashboard, workspace, and fleet events over SSE; commands return
through authenticated API routes. Optional host agents keep monitoring on an
authenticated read-only protocol and expose remote sessions only through a
second, explicitly enabled control credential. Grok credentials and host-agent
tokens never enter the browser.

Longer design notes live in [docs/architecture.md](docs/architecture.md).
Versioned delivery plans live in [docs/roadmap.md](docs/roadmap.md).

## Stack

Versions come from [`package.json`](package.json). Engines require **Node.js 22
or newer**. CI verifies Node 22 and 24.

| Layer | From the manifest |
| --- | --- |
| Runtime | Node.js `>=22` |
| UI | React `^19`, Vite `^8`, TypeScript `^7` |
| API | Express `^5` |
| Agent control | `@agentclientprotocol/sdk` `^1.3` |
| File watching | chokidar `^5` |
| Unit tests | Vitest `^3` |
| Browser tests | Playwright `^1` |
| Dev orchestration | concurrently `^10` |

## Dashboard

Twelve primary views, plus Session Console overlays:

| View | What you see |
| --- | --- |
| **Live** | Active Grok processes, event stream, bounded runtime intelligence |
| **Control** | New and resumed ACP lanes, approvals, stop/resume |
| **Runs** | Cross-session workflow field and safe local controls |
| **Changes** | Live Git status and bounded diffs |
| **Overview** | Session counts, tools, model mix, workspaces |
| **Sessions** | Searchable local archive, active vs archived |
| **Activity** | Two-week turns, tools, line movement, errors |
| **Usage** | Provenance-aware ledger, budgets, JSON/CSV export |
| **Fleet** | Registered hosts, remote telemetry, opt-in remote sessions |
| **Library** | Local skills, agent profiles, marketplace plugin names |
| **Memory** | Metadata-only inventory of durable Markdown memory |
| **Themes** | Operator, Event Horizon, Minimal Calm |

A command palette (`⌘ K` / `Ctrl K`, or `/`) jumps to a view or local session.
Number keys and the `u` / `f` shortcuts switch views. Optional browser
notifications fire when a live agent or permission queue needs input.

## Quickstart

### Requirements

- Node.js 22 or newer
- A working [`grok`](https://github.com/xai-org/grok-build) installation
- Grok Build account access

### One-command launch

The packaged release checks its environment, starts the local server, and opens
the dashboard:

```bash
npx --yes grok-ui
```

Run diagnostics without starting the dashboard:

```bash
npx --yes grok-ui doctor
```

Start the read-only agent on a machine you want to monitor:

```bash
GROK_UI_AGENT_TOKEN='<separate-long-random-token>' \
npx --yes grok-ui agent
```

The agent always requires its own token, including on loopback, and defaults to
`127.0.0.1:4311`. Use `--host`, `--port`, `--grok-home`, or `--state-dir` after
`agent` when that machine needs an override. The central Fleet view stores the
matching token server-side when you register the host.

To opt that host into secure remote sessions, provide a second, different
token:

```bash
GROK_UI_AGENT_TOKEN='<monitoring-token>' \
GROK_UI_AGENT_CONTROL_TOKEN='<different-control-token>' \
npx --yes grok-ui agent
```

In the central Fleet editor, enter the same control token and enable Remote
Sessions for that host. A healthy, fresh host can then start Grok in a workspace
it has already observed. The Remote Session Console streams the conversation,
accepts natural-language follow-ups, presents Grok's exact permission choices,
and can interrupt the current turn. It is not a remote terminal and exposes no
arbitrary shell.

### Run from source

```bash
git clone https://github.com/joeynyc/Grok-UI.git
cd Grok-UI
npm ci
npm run doctor
npm start
```

`npm start` creates the production build automatically when it is missing,
then serves `dist/` and `dist-server/`. For file-watching development, use
`npm run dev` (Express API plus Vite; Vite proxies `/api` to the local API).

The first launch opens a live setup diagnostic that distinguishes a missing
CLI, an unauthenticated account, and a profile that simply has not created its
first session yet. Machine-specific paths are never included in those checks.

### CLI options

| Option | Purpose |
| --- | --- |
| `doctor` | Check Node, Grok CLI, authentication, and local state |
| `agent` | Start the authenticated host agent; remote sessions are opt-in |
| `--port <number>` | Override dashboard port `4310` or agent port `4311` |
| `--host <address>` | Override the loopback bind address |
| `--grok-home <path>` | Use a specific Grok state directory |
| `--state-dir <path>` | Use a specific private Grok UI state directory |
| `--no-open` | Start without opening a browser |
| `--help` | Print CLI usage |
| `--version` | Print the installed Grok UI version |

## Live runtime

Start `grok` in any workspace and the session appears as soon as its live
process registers. The runtime view projects:

- agent state: working, waiting, idle, or needs input
- current phase and active tool
- structured user, assistant, reasoning, plan, and tool events
- turns, tool calls, context usage, and reported cost
- process identity and workspace

Process liveness is rechecked continuously, so “live now” means an open Grok
process—not a recently modified history file.

## Session control

Grok UI supervises `grok agent --no-leader stdio` and communicates through
the official [Agent Client Protocol](https://agentclientprotocol.com/).

It supports new sessions, `session/load`, prompts, permission requests,
confirmed cancellation, and independently running managed lanes. Stop also
cancels pending permissions, preserves final tool updates, surfaces a retry when
Grok does not confirm, and leaves the lane ready to resume. Rename and archive
actions are local Grok UI overlays; Grok’s own session files are never rewritten.

## Session previews

The local Session Console detects `dev` or `start` scripts in the workspace
already associated with that session and shows the exact command before
anything runs. Starting a preview launches a separate process without a shell,
binds supported frameworks to a random loopback port, and streams a bounded
output tail into the console.

The preview surface includes desktop, tablet, and mobile widths, reload and
external-open controls, and an explicit Stop action. Preview processes are
ephemeral and terminate when Grok UI shuts down. Privacy Mode hides the embedded
application and redacts the displayed command and logs.

## Workflow runs

The Runs view listens for Grok’s structured `workflow_updated` notifications on
UI-managed ACP sessions and aggregates them across the command field. Each run
can show:

- current status, objective, phase progression, and latest event
- per-agent state, phase, model, duration, and token usage
- active, used, reserved, and total agent allocation
- derived run-wide token totals with explicit incomplete-usage states
- pause context and final result summary
- Pause, Resume, and Stop controls when the run state supports them

Controls use Grok’s documented `/workflow pause|resume|stop <display-name>`
commands and accept only validated display handles. Persisted snapshots remain
visible after a dashboard restart, but their controls are disabled because Grok
only resumes failed or paused workflows inside the process that owns them.

Grok UI intentionally does not scrape the terminal dashboard or imply visibility
into historical CLI-only workflow runs. A run appears after a UI-managed session
emits its first workflow update.

## Usage ledger

The v0.9 Usage view extends the existing token and cost telemetry with a
durable local ledger. It reports by project, model, session, agent, and time
period, while keeping session totals separate from workflow-agent detail.

Every value says whether it is Grok-reported, derived, incomplete, or
unavailable. Grok UI does not substitute context-window occupancy for
cumulative CLI token usage, and explicitly mixed observation scopes are never
presented as precise spend.

Optional token or reported-cost budgets can be scoped globally or to a project,
model, session, or workflow agent. They are evaluated only against a
non-overlapping ledger scope, and local alerts are deduplicated at 80% and
100%. JSON and CSV exports are capped, authenticated, and redacted on the
server before download whenever Privacy Mode is active.

## Runtime intelligence

The Live view projects bounded process trees from active Grok and UI-managed
process IDs. It discovers listening TCP ports only for that selected process
set, classifies common databases and development services without connecting
to them, and shows structured test status and external-call categories from the
existing safe tool lifecycle.

Inspection uses fixed timeouts, output, process-count, and tree-depth caps. Raw
command arguments, tool input, credentials, headers, terminal history, and
unrelated host processes are never included in the browser contract.

## Multi-machine monitoring

The v0.10 Fleet view monitors up to 32 explicitly registered hosts. Each host
reports a stable identity, Grok UI, agent, and Grok versions, negotiated read
capabilities, health, round-trip latency, last-seen time, and freshness.
Bounded tabs show remote sessions, workflow runs, runtime state, and
provenance-aware usage.

Hosts can be reached in three constrained ways:

- **SSH** — Grok UI manages a fixed loopback forwarding process with
  argument-separated `ssh` options, batch mode, no shell, and no remote command.
- **Tailscale** — the registry accepts only `.ts.net` names or addresses from
  Tailscale's IPv4 or IPv6 ranges.
- **Direct** — an advanced/test transport restricted to HTTP or HTTPS loopback
  origins.

The monitor polls every 5 seconds with a 3.5-second request timeout and at most
four concurrent remote reads globally, including explicit refresh and detail
requests. Per-agent responses are capped at 2 MiB and the central fleet
projection at 4 MiB. A sample becomes stale after 15 seconds and offline after
45 seconds. Connecting, healthy, degraded, stale, offline, incompatible,
unauthorized, and unavailable states remain distinct in the UI.

v0.10 is strictly read-only on remote machines. It cannot remotely start work,
send prompts, approve permissions, pause, resume, interrupt, stop, execute a
shell, write files, or probe arbitrary endpoints. Registering, editing,
refreshing, or removing a host changes only the central monitor configuration
or triggers a read.

## Secure remote sessions

v0.11 keeps all v0.10 monitoring routes read-only and adds a separate,
host-authorized control channel. It lets a user start or continue a real
ACP-managed Grok conversation from another device, send follow-ups, answer
Grok's permission requests, and interrupt a running turn.

Every mutation carries a durable command ID and bounded delivery expiry.
Retrying the same delivery cannot silently create duplicate work, and a host
restart turns an ambiguous command into an explicit reconciliation state
instead of replaying it. Existing CLI-observed sessions remain read-only;
follow-up control is limited to host-managed sessions. Controls remain
unavailable while the host is stale, unhealthy, incompatible, disabled, or
missing the exact capability.

The initial scope does not add arbitrary shell access, remote workflow
Pause/Resume/Stop, team roles, or shared approvals. See
[`docs/v0.11-secure-remote-sessions.md`](docs/v0.11-secure-remote-sessions.md)
for the product contract and staged decisions.

## Themes

Three complete visual systems ship with the dashboard:

- **Operator** — carbon black, signal lime, and a precision HUD grid
- **Event Horizon** — deep-space red, cold starlight, and glass command surfaces
- **Minimal Calm** — quiet stone surfaces, sage signals, and restrained motion

Theme selection stays in local browser storage and never changes session data.

## Privacy and security

- The server binds to the loopback interface by default.
- Non-loopback binding requires `GROK_UI_TOKEN`.
- Grok credentials never pass through the browser.
- Local preview processes bind to loopback, receive no Grok credentials, and
  start only after an explicit user action.
- Persistent Privacy Mode replaces visible session names, paths, identifiers,
  event content, file names, remote host names, and endpoints with stable
  presentation-safe aliases.
- Privacy Mode protects recordings and screen shares; it is not an access-control
  boundary. Authorized browser clients can still receive the underlying local data.
- Authentication cookies are `HttpOnly` and `SameSite=Strict`.
- API responses are non-cacheable and include restrictive security headers.
- Raw system prompts and durable-memory bodies are not indexed.
- File, diff, and event reads are bounded.
- Host-agent reads require a dedicated bearer token, reject redirects, time out
  after 3.5 seconds, and cap responses at 2 MiB.
- Fleet URLs are restricted by transport, and the connector accepts only fixed
  `/agent/v1/` protocol paths.
- Diff access is limited to the dashboard process working directory and
  workspaces associated with known sessions.
- Grok UI contains no analytics or product telemetry.
- Repository screenshots and diagrams use sanitized demo data only.

For remote use, set a long random token, place TLS or a private tunnel in
front of the server, and bind only to the interface you intend to expose:

```bash
HOST='<bind-address>' \
GROK_UI_TOKEN='<long-random-token>' \
npm start
```

See [SECURITY.md](SECURITY.md) for the full deployment and reporting guidance.

## Configuration

These environment variables are read by the CLI, doctor, dashboard, and host
agent. Do not put secrets in the repository. Non-loopback dashboard binds
require `GROK_UI_TOKEN`. Host-agent mode always requires `GROK_UI_AGENT_TOKEN`.

| Variable | Purpose |
| --- | --- |
| `HOST` | Dashboard API bind interface |
| `PORT` | Dashboard API listen port |
| `GROK_HOME` | Grok state directory |
| `GROK_BIN` | Grok executable used by the ACP controller and doctor |
| `GROK_UI_TOKEN` | Dashboard access token; required when `HOST` is not loopback |
| `GROK_UI_STATE_DIR` | Private annotations, usage state, fleet registry, and command evidence |
| `GROK_UI_AGENT_TOKEN` | Dedicated bearer token for host-agent monitoring |
| `GROK_UI_AGENT_CONTROL_TOKEN` | Distinct token that enables secure remote sessions on the host |
| `GROK_UI_AGENT_HOST` | Host-agent bind interface |
| `GROK_UI_AGENT_PORT` | Host-agent listen port |
| `GROK_UI_AGENT_LABEL` | Optional host label advertised by the agent |

CLI flags `--host`, `--port`, `--grok-home`, and `--state-dir` set the matching
variables for one process (`GROK_UI_AGENT_HOST` / `GROK_UI_AGENT_PORT` in
`agent` mode).

## Commands

```bash
npm run dev           # API (tsx watch) + Vite with file watching
npm run doctor        # Check Node, Grok CLI, authentication, and local state
npm run setup         # Run preflight checks and create a production build
npm run check         # Type-check client and server
npm test              # Vitest unit and integration tests under server/ and src/
npm run test:e2e      # Build, then Playwright onboarding, control, and fleet tests
npm run test:e2e:report # Open the last Playwright HTML report
npm run test:soak     # 75-second managed-session soak (also used by tagged releases)
npm run build         # Produce client (dist/) and server (dist-server/) builds
npm run verify        # check + test + build + release:check
npm run release:check # Audit the exact npm pack contents
npm run test:package  # Install and launch the packed artifact in isolation
npm start             # Build when needed, then serve the production app
npm run agent         # Serve the built host agent; control remains opt-in
npm run serve         # Serve an existing production build without rebuilding
```

`npm test` covers parsing, ACP control, workflow projection, usage, fleet
protocol, workspace, security, setup, and Privacy Mode. `npm run test:e2e`
starts a production server against a fake `grok` binary; it does not call a
real model. Set `PLAYWRIGHT_BROWSER=webkit` to run the Safari engine locally.
CI runs Chromium and WebKit.

## Continuous integration

[`.github/workflows/ci.yml`](.github/workflows/ci.yml) runs on pushes to
`main` and on pull requests:

- **verify** — `npm run verify` and a high-severity production audit on Node 22 and 24
- **package-smoke** — isolated pack/install/launch on Ubuntu and macOS
- **browser-e2e** — Playwright Chromium and WebKit against the production build

Dependabot watches npm and GitHub Actions monthly.

## Deploy and release

Grok UI is not a cloud app. Typical use is local:

1. `npx --yes grok-ui` or `npm start` on the machine that owns `GROK_HOME`.
2. Open the printed localhost URL.
3. For another device, use an SSH tunnel or a private Tailscale network. Set
   `GROK_UI_TOKEN` before binding the dashboard beyond loopback, and terminate
   TLS in front of the process. Do not expose the listen port to the public
   internet.
4. To monitor or control another machine, run `grok-ui agent` there and
   register it in Fleet. See [Secure remote sessions](#secure-remote-sessions).

The public npm package is [`grok-ui`](https://www.npmjs.com/package/grok-ui).
Pushing an annotated `v*` tag runs
[`.github/workflows/release.yml`](.github/workflows/release.yml): verify,
audit, Playwright, soak, package smoke, `npm pack`, trusted npm publish, and a
GitHub Release that attaches the tarball. See
[docs/releasing.md](docs/releasing.md).

## Repository map

```text
bin/grok-ui.mjs           packaged CLI (start, agent, doctor, flags)
scripts/                  start, doctor, release-check, package-smoke, e2e, soak
server/
  index.ts                Express app, SSE, API routes
  grok-controller.ts      ACP lifecycle, prompts, approvals, cancellation
  workflow-state.ts       workflow notification projection and safe controls
  live-monitor.ts         active process and runtime event projection
  runtime-inspector.ts    bounded process-tree, port, and service inspection
  usage-ledger.ts         provenance-aware durable usage reporting
  usage-budgets.ts        local token/cost budgets and deduplicated alerts
  usage-export.ts         authenticated JSON/CSV usage export
  session-projection.ts   shared local session-to-row projection
  host-agent.ts           read-only monitoring and opt-in session control
  host-agent-entry.ts     standalone host-agent process entry
  remote-command-store.ts durable command reconciliation and audit evidence
  fleet-protocol.ts       versioned parsing, caps, and host namespacing
  fleet-registry.ts       atomic private connection registry
  fleet-connectors.ts     bounded direct, Tailscale, and SSH transports
  fleet-monitor.ts        polling, compatibility, health, and freshness
  grok-store.ts           historical metadata, library, and memory index
  session-reader.ts       bounded conversation and tool timeline
  session-state.ts        durable managed lanes, usage, and annotations
  preview-supervisor.ts   loopback web preview lifecycle and bounded logs
  workspace-inspector.ts  live Git status and bounded diff inspection
  setup-diagnostics.ts    first-run / doctor-equivalent setup checks
  security.ts             local and remote access gate
src/
  App.tsx                 dashboard shell, views, event-stream client
  privacy.tsx             presentation-only Privacy Mode aliases
  views/ControlView.tsx   command deck and approval queue
  views/WorkflowsView.tsx cross-session workflow command field
  views/UsageView.tsx     time- and dimension-based usage ledger
  views/FleetView.tsx     fleet page coordination and registry actions
  views/fleet/            status, editor, selectors, and telemetry panels
  views/ChangesView.tsx   live repository change workbench
  views/SessionWorkbench.tsx
                          local Session Console (timeline, preview, operations)
  views/RemoteSessionWorkbench.tsx
                          live remote conversation and safe controls
  views/RuntimeIntelligencePanels.tsx
                          Live-view process, port, test, and call panels
e2e/                      Playwright journeys against a fake Grok CLI
docs/                     architecture, roadmap, milestone records, artwork
public/                   favicon, brand mark, theme assets
```

Build outputs `dist/` (Vite client) and `dist-server/` (compiled API and host
agent). Those directories are what `npm start`, `npm run serve`, `npm run agent`,
and the published package run.

The longer design and trust-boundary notes live in
[docs/architecture.md](docs/architecture.md). Concrete edit routing and safety
invariants live in [docs/AGENT_GUIDE.md](docs/AGENT_GUIDE.md).

## Project status

Grok UI is a community project. Current tree version is **0.11.0**. Its local
monitor, control, Session Console, runtime, usage, Git inspection,
multi-machine monitoring, and secure remote-session paths are functional and
covered by automated tests. Packed releases are installed and launched in
isolation on macOS and Linux CI. Grok Build, ACP, and the fleet protocol can
evolve, so compatibility fixes may be needed for future releases.

## License

MIT — see [LICENSE](LICENSE).
