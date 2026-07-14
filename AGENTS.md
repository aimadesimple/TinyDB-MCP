# Repository Guidelines

## Project Structure

`tinydb_remote_server.py` is the application entry point. It configures the
FastMCP Streamable HTTP server and defines the TinyDB-backed MCP tools.
Project metadata and runtime dependencies are in `pyproject.toml`; `uv.lock`
must stay synchronized with them. `NOTES.md` documents setup, GitHub
publishing, Render deployment, and MCP Inspector use. TinyDB writes local
runtime state to `db.json`, which is intentionally ignored by Git. Add future
automated tests under `tests/`.

## Development Commands

- `uv sync` installs the locked dependencies into the project environment.
- `uv run --frozen python tinydb_remote_server.py` starts the server. Locally,
  it listens on `0.0.0.0:10000` unless `PORT` is set.
- `uv run --frozen python -m py_compile tinydb_remote_server.py` checks Python
  syntax without starting the server.
- `uv run pytest` runs tests once `pytest` is added as a development dependency.

Use `uv add <package>` for runtime dependencies so both `pyproject.toml` and
`uv.lock` are updated together. Do not hand-edit `uv.lock`.

## Code Style and MCP Tools

Target Python 3.13+, four-space indentation, standard import grouping, and
type annotations for tool parameters and return values. Use `snake_case` and
verb-led tool names, for example `insert_data` and `delete_all_data`. Give each
`@mcp.tool()` a short docstring describing inputs, output, and side effects.
Keep the public HTTP configuration intact: bind to `0.0.0.0` and read the port
from `PORT`, with `10000` only as a local fallback.

## Testing

Write `pytest` tests named `test_<behavior>.py` or `test_<behavior>()`, such as
`test_insert_data_returns_success`. Use a temporary TinyDB file or fixture;
tests must never read or modify root-level `db.json`. Cover successful calls,
empty-database behavior, and destructive operations.

## Commits, Pull Requests, and Deployment

Recent commits use short, imperative subjects, such as `Configure Render`
and `Fixed insert_data tool description`. Keep commits focused and include
validation in the PR description; link issues and include MCP request/response
examples when an interface changes. The public GitHub remote is
`AIMadeSimple/TinyDB-MCP`; push reviewed changes to `main` with `git push`.
Render deploys from `main` using the commands documented in `NOTES.md`.

## Security and Data

Never commit `.env` files, credentials, or `db.json`. The current Render
service is public and unauthenticated, and its Free-tier filesystem is
ephemeral. Do not store sensitive or durable production data there.
