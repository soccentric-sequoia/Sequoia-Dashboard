# Sequoia-Dashboard

## Project Summary

Sequoia-Dashboard is the operator-facing web console for the Sequoia IV&V workflow. The repository
is organized around the `webui` package and includes the package lockfile, contract-test tooling,
frontend source, server API/SSE findings, and an independent audit. The UI is intended to consume
the Sequoia Server REST API and documented run and log event streams rather than inventing a second
backend contract. Its operator surface covers dashboard navigation, connection state, test-run
control, run status, settings, logs, and live updates. The checked-in web package documentation and
TypeScript/React sources are the authoritative implementation surface; the root README previously
linked those materials without describing the product.

Contract tests exercise smoke behavior, settings, runs, and streaming through a Python test project.
Frontend unit tests cover utility behavior and the live-run reducer. The repository also carries
requirements and audit documents that record intended behavior and known findings. Because UI
rendering, browser connectivity, authentication, and a live Sequoia-Server are environment
dependent, this README distinguishes source/test evidence from manual verification. The package
manager and exact scripts should be taken from `webui/package.json`; the contract-test Makefile
provides the API-level entry point. No claim is made here that the console has been validated
against a deployed server or real browser session.

The audit and findings documents are retained as navigation points because they record the API
assumptions and known gaps behind the UI. Live SSE behavior can differ from a static contract test
when reconnects, expired sessions, or partial run data are involved. The dashboard is therefore an
operator client whose release depends on the matching server contract.

## Feature Inventory

| # | Name / Description | Implemented? | Test Coverage? | Manual Test |
| --- | --- | --- | --- | --- |
|   1 | Web UI package and operator shell        | Implemented  | Frontend tests present                          | [ ] Manual verification required |
|   2 | Navigation and operator views            | Implemented  | Partial; component coverage limited             | [ ] Manual verification required |
|   3 | Connection/health state presentation     | Partial      | Utility/reducer coverage only                   | [ ] Manual verification required |
|   4 | Start/stop run controls                  | Implemented  | Contract run tests present                      | [ ] Manual verification required |
|   5 | Run status and live-run reducer          | Implemented  | Covered by `webui/lib/live-run/reducer.test.ts` | [ ] Manual verification required |
|   6 | Settings/configuration view              | Implemented  | Covered by contract settings test               | [ ] Manual verification required |
|   7 | Log and SSE stream consumption           | Implemented  | Covered by contract streaming test              | [ ] Manual verification required |
|   8 | Server REST API integration              | Implemented  | Contract tests in `contract-tests/tests/`       | [ ] Manual verification required |
|   9 | Smoke/contract harness                   | Implemented  | `test_01_smoke.py` and related tests            | [ ] Manual verification required |
|  10 | Authentication and production deployment | Unknown      | No deployment verification found                | [ ] Manual verification required |

## Validation

Inspect `webui/package.json` for package scripts and run the repository’s contract-test Makefile
from `contract-tests/`. The focused test files are `test_01_smoke.py`, `test_02_runs.py`,
`test_03_settings.py`, `test_04_streaming.py`, `webui/lib/utils.test.ts`, and
`webui/lib/live-run/reducer.test.ts`.

## Documentation

- [Web UI package README](webui/README.md)
- [Requirements](docs/req.md)
- [API findings](docs/WEB_FINDINGS.md)
- [Audit](docs/AUDIT.md)
- [docs/req.md](docs/req.md) — original design/build prompt for the Web UI.
- [docs/WEB_FINDINGS.md](docs/WEB_FINDINGS.md) — verified Server REST/SSE API inventory.
- [docs/AUDIT.md](docs/AUDIT.md) — independent verification audit of this repo.
- [.claude/skills/dashboard-architecture/SKILL.md](.claude/skills/dashboard-architecture/SKILL.md) —
  architecture, frontend/backend communication, and UI conventions for coding agents.
