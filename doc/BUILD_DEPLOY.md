# Sequoia-Dashboard Build and Deployment

## Scope

Sequoia-Dashboard is the `webui` Next.js application. The root Makefile wraps
the scripts in `webui/package.json` and delegates container operations to
`docker/Makefile`. The package manifest provides Next.js development/production
commands, ESLint, TypeScript, and Vitest.

## 1. Prerequisites

- Node.js/npm compatible with the checked-in Next.js and React versions.
- Docker for image and container targets.

```sh
make install                 # npm install in webui/
make help
```

## 2. Setup

## 3. Build

```sh
make dev                     # Next dev server, normally port 3010
make build                   # Next production bundle in webui/.next
```

The port is configurable for Docker/run targets with `PORT=N`; local Next.js
development follows the package script's defaults unless configured in Next.js.

## 4. Test and lint

```sh
make test                    # webui: npm test, Vitest run
make lint                    # webui: npm run lint, ESLint
cd webui && npm run type-check  # package manifest target; no root Make target
```

## 5. Artifacts

```sh
make docker-build IMAGE=sequoia-dashboard TAG=latest
```

The local production bundle is `webui/.next`. Docker packaging creates the
configured image (default `sequoia-dashboard:latest`) through `docker/Makefile`;
the exact image contents and runtime environment are defined by `docker/Dockerfile`.

## 6. Deploy, install, and run

```sh
make docker-run PORT=3010 IMAGE=sequoia-dashboard TAG=latest
make docker-logs IMAGE=sequoia-dashboard
make docker-shell IMAGE=sequoia-dashboard
make docker-stop IMAGE=sequoia-dashboard
```

These targets run a local Docker container. No cloud credentials or remote
deployment target is declared. Configure application backends and environment
variables according to the deployment environment before treating the UI as
an integrated dashboard.

## 7. Cleanup

```sh
make clean                   # remove webui/.next
make docker-clean IMAGE=sequoia-dashboard TAG=latest
```

## 8. Verification caveats

Unit tests and linting do not prove API connectivity, authentication, browser
compatibility, or a device-backed workflow. Docker validation is local and does
not deploy to a cloud service. Manually verify the built dashboard against the
actual Sequoia service endpoints and representative target-device data.