# Livebook tester environment

Baseline: upstream `v0.19.9` at `782dc5ecfe103e953c8157df1b47613896aa2c1e`.

## Commands

```bash
./tester-env deploy
./tester-env seed
./tester-env verify
./tester-env reset
```

`deploy` builds the editable upstream Dockerfile with `BASE_IMAGE=hexpm/elixir:1.19.3-erlang-28.1.1-ubuntu-noble-20251013` and `VARIANT=default`, then starts Livebook at `http://localhost:18082` (iframe assets: `18083`). It uses the fixed password `livebook-tester-password`, disables tokens, and uses the fixed test secret in `docker-compose.tester-env.yml`.

`seed` replaces `/data/notebooks` with the four committed fixtures: Weekly Delivery Review, Release Readiness, Incident Follow-up, and nested-tabs-check. `verify` checks `/public/health` and that those exact four non-empty files exist. `reset` runs `docker compose down --volumes --remove-orphans` for this run only; it preserves the source image and never touches shared Chromium infrastructure.

`IMAGE_TAG` selects the image name (default `tester-env-livebook:dev`). `RUN_ID` scopes only the Compose project, container, and volume names; it is not included in the image name.

## Recorded evidence

On 2026-09-06, two clean `reset -> deploy -> seed -> verify` cycles passed. Browser MCP authenticated at the HTTP endpoint, showed the three seeded notebooks in storage, and opened Weekly Delivery Review; evaluating its built-in Elixir cell visibly rendered `{3, "Accounts API, Billing Worker, Customer Portal"}` with green `Evaluated` status.
