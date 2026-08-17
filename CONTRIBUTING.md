# Contributing

Thanks for helping improve Grok UI.

## Development

1. Install Node.js 22 or newer and Grok Build.
2. Run `npm ci` (or `npm install`) at the repository root.
3. Run `npm run doctor` if you will talk to a real `grok` install.
4. Run `npm run dev` (Express API + Vite; Vite proxies `/api` to the API).
5. Before submitting a change, run `npm run verify`.
6. For UI or onboarding changes, also run `npm run test:e2e`.
7. For packaging or startup changes, also run `npm run test:package`.
8. For host-agent or fleet changes, also run `npm run test:e2e`,
   `npm run test:soak`, and `npm audit --omit=dev --audit-level=high`.

Keep the server loopback-only during development. Tests must not depend on real
Grok credentials or make model requests. Playwright uses
`scripts/fake-grok-e2e.mjs` and a temporary Grok home. CI runs Chromium and
WebKit; locally, `PLAYWRIGHT_BROWSER=webkit` selects Safari.

User-facing copy should say **Open Session** and **Session Console**. The
source module may still be named `SessionWorkbench`.

See [docs/AGENT_GUIDE.md](docs/AGENT_GUIDE.md) for edit routing and invariants.

## Pull requests

- Keep changes focused and explain the user-facing impact.
- Add tests for parsing, permissions, path validation, and security boundaries.
- For fleet changes, test protocol compatibility, authentication failures,
  bounded timeouts, disconnect and reconnect, freshness transitions, strict
  monitoring/control credential separation, exact route and capability gates,
  idempotent delivery, and honest unknown outcomes.
- Include desktop and mobile verification for UI changes.
- Never commit `.env`, `~/.grok` data, session transcripts, credentials, or screenshots containing private source or prompts.
- Keep package contents under the release-check size and privacy limits.

## Design principles

- Prefer real Grok/ACP state over simulated UI.
- Preserve explicit user approval for sensitive tools.
- Keep live reads bounded.
- Make offline, empty, and failure states understandable.
- Maintain the black-box flight-recorder visual language.

See [docs/releasing.md](docs/releasing.md) for the tagged release process.
