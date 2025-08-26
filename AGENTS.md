# Repository Guidelines

## Project Structure & Module Organization
- app entrypoint: `main.py` (FastAPI app + APScheduler job).
- alt scripts: `schedule.py`, `test.py` (dev/legacy variants for ingestion).
- dependencies: `requirements.txt`.
- cache/build artifacts: `__pycache__/` (do not commit).

## Build, Test, and Development Commands
- create venv: `python -m venv .venv && source .venv/bin/activate`.
- install deps: `pip install -r requirements.txt`.
- run API locally: `uvicorn main:app --reload --host 0.0.0.0 --port 8000`.
- health check: `curl http://localhost:8000/health`.
- format (optional if installed): `black .` and `isort .`.

## Coding Style & Naming Conventions
- style: PEP 8, 4‑space indents, type hints for new/changed functions.
- naming: files/modules `snake_case.py`; constants `UPPER_SNAKE_CASE`; functions/vars `lower_snake_case`.
- FastAPI: keep a single `app` in `main.py`; prefer dependency‑injected helpers over globals.
- database: use `sqlalchemy.text(...)` with named binds; restrict inserts to allowed columns.

## Testing Guidelines
- framework: no formal suite yet; prefer `pytest` for new tests.
- layout: place tests in `tests/` mirroring module names, e.g., `tests/test_ingestion.py`.
- run (if added): `pytest -q`.
- coverage: target critical paths (ACRA fetch pagination, upsert query building, health endpoint).

## Commit & Pull Request Guidelines
- commits: concise imperative subject (<= 72 chars), e.g., `ingest: filter Live records and upsert allowed columns`.
- scope: group related changes; separate refactors from behavior changes.
- PRs must include: summary, rationale, screenshots/logs if relevant, env/config changes, and rollback notes.
- link issues: reference `#id` in the description.

## Security & Configuration Tips
- required env vars: `DATABASE_URL`, `API_URL`, `RESOURCE_ID`, `PAGE_SIZE`.
- local `.env` example:
  - `DATABASE_URL=postgresql://user:pass@host:port/db?sslmode=require`
  - `API_URL=https://data.gov.sg/api/action/datastore_search`
  - `RESOURCE_ID=<dataset_id>`
  - `PAGE_SIZE=100`
- secrets: never commit real credentials; use `.env` locally and deployment secrets in CI/hosting.

