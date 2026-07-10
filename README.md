# Agent.Common

Shared foundation primitives — cancellation, eventing, DI container, logging, JWT/db
helpers, telemetry — reused across the sibling repos in this workspace: [Agent.Local](../Agent.Local)
(the coding agent) and [Agent.Workflow](../Agent.Workflow) (auth/host/Temporal/web).

Published as `@agent-local/common`. Consumers depend on it via a `file:` reference
(`"@agent-local/common": "file:../../../Agent.Common"` from a package two levels deep
under `src/`), so this repo must sit beside the consuming repo and be built before it.

---

## What's in here

| Module | Purpose |
| --- | --- |
| `lifetime` | `CancellationToken` — the one-root-signal-per-request cancellation model (`AbortSignal.any([parent, own])`). |
| `eventing` | The root event bus contract used to wire cross-cutting notifications. |
| `container` | A minimal DI container for registry-based composition. |
| `ilogger` / `filelogger` | Logging interface + a file-backed implementation. |
| `appsettings` | Typed settings loading. |
| `jwt` | JWT sign/verify helpers built on `jose`. |
| `db` | SQLite (better-sqlite3) access helpers. |
| `comms` | Shared wire contracts for the host↔agent HTTP round-trip. |
| `tokenusage` | Token accounting helpers. |
| `telemetry` | OpenTelemetry logger/metrics registry. |

## Install and build

```bash
npm install
npm run build     # tsc -p tsconfig.json
npm test          # jest
```

## Versioning note

This package has no independent release process yet — it's consumed by file path,
so a change here takes effect in a consumer the next time that consumer runs
`npm install` (npm re-links the `file:` dependency) or, for `file:` deps, sometimes
just on rebuild. Keep exports backward compatible with both consumer repos in mind,
since they aren't updated in lockstep.
