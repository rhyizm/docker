# Repository Guidelines

## Project Structure & Module Organization
Docker assets live in sibling directories so you can focus on a single stack at a time. `py313-slim-bookworm/` and `py313-alpine/` each hold one Dockerfile plus optional `requirements.txt`, and are tuned respectively for Debian Bookworm (Playwright-capable) and Alpine (minimal tooling). `mongodb/` and `mysql-57/` provide turnkey Compose files for local databases with data volumes already mapped. `mcp-servers/` contains the Playwright MCP stack (`docker-compose.yml` plus `playwright.config.json`). Global docs live in `README.md`; add any shared scripts to the repo root to keep image folders self-contained.

## Build, Test, and Development Commands
- Build the Debian image: `docker build -t py313-slim-bookworm:python3.13-slim-bookworm -f py313-slim-bookworm/Dockerfile py313-slim-bookworm`
- Build the Alpine image: `docker build -t py313-alpine:python3.13-alpine -f py313-alpine/Dockerfile py313-alpine`
- Launch MongoDB: `docker compose -f mongodb/docker-compose.yml up -d` (data persists in `mongo-data`)
- Launch MySQL 5.7: `docker compose -f mysql-57/docker-compose.yml up -d` (volume `mysql-data`)
- Start the Playwright MCP server: `docker compose -f mcp-servers/docker-compose.yml up --build`
Use `docker compose logs -f <service>` for runtime validation and `docker image prune` after iterative builds.

## Coding Style & Naming Conventions
Keep Dockerfiles declarative: group `RUN` directives by purpose, chain commands with `&& \`, and leave concise comments (Japanese is already used for ops notes). Environment variables stay uppercase, while image tags follow `name:python<version>-<base>` for clarity. Sort package lists logically (base tools, build deps, runtime libs) and prefer explicit versions in `ARG VARIANT` so rebuilds remain deterministic. Add new Compose services in their own folders named after the stack (`redis-65`, etc.) so manifests stay discoverable.

## Testing Guidelines
Run `docker build` with `--no-cache` at least once per change to catch missing dependencies. After building Python images, execute `docker run --rm <tag> python --version` and, when applicable, `python -c "import playwright"` or `pip check` to confirm critical modules. For Compose stacks, script smoke tests such as `docker exec mongodb mongosh --eval 'db.runCommand({ ping: 1 })'` or `docker exec mysql-57 mysqladmin ping -ptestpass`. Keep test scripts beside the corresponding Compose file and prefix them with `test-` (e.g., `mongodb/test-health.sh`).

## Commit & Pull Request Guidelines
Use short, imperative commit subjects scoped by directory, for example `py313-slim-bookworm: add libheif deps`. Describe motivation and notable commands in the body when configuration changes might impact consumers. Pull requests should mention the affected image/service, include sample build or Compose output, and reference linked tickets or docs. Add screenshots or logs only when they clarify runtime behavior (e.g., MCP server startup banner).

## Security & Configuration Tips
Never commit real credentials; the Compose files default to obvious strings (`testpass`, `QazxSw`) so override them via `.env` or CI secrets before deployment. Keep host volumes on encrypted disks if they may hold customer data, and scrub `docker system prune` outputs before sharing logs. Review upstream base images monthly for CVEs and bump `VARIANT` tags in lockstep across Dockerfiles to minimize drift.
