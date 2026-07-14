# Repository Guidelines

## Project Structure & Module Organization

This Python MCP server uses `tinydb_remote_server.py` as its entry point; it creates the FastMCP server, opens TinyDB, and declares exposed tools. Dependencies and Python metadata live in `pyproject.toml`; locked versions are in `uv.lock`. `NOTES.md` holds setup notes. Runtime data is written to root-level `db.json`—treat it as local state, not source code. There is no `tests/` directory yet; add tests there as the server grows.

## Build, Test, and Development Commands

- `uv sync` installs the locked project dependencies into the local virtual environment.
- `uv run python tinydb_remote_server.py` starts the FastMCP server using its streamable HTTP transport.
- `uv run python -m compileall tinydb_remote_server.py` performs a quick syntax check.
- `uv run pytest` runs the test suite after tests are added. Add `pytest` as a development dependency before relying on this command.

Use `uv add <package>` to introduce a runtime dependency so that both `pyproject.toml` and `uv.lock` stay current.

## Coding Style & Naming Conventions

Use Python 3.13+, four-space indentation, standard-library import ordering, and type annotations for tool inputs and returns. Follow PEP 8 naming: `snake_case` for functions and variables, `PascalCase` for classes, and verb-led tool names such as `insert_data` or `delete_all_data`. Keep MCP tool docstrings concise and document arguments, returns, and persistent side effects. Avoid changing the server transport or `db.json` location without documenting migration implications.

## Testing Guidelines

Write `pytest` tests in `tests/` with names such as `test_insert_data_returns_success`. Isolate database state with a temporary TinyDB path or fixture; tests must not depend on or mutate the root `db.json`. Cover successful operations and empty-database behavior for every MCP tool. Run the full suite with `uv run pytest` before opening a change.

## Commit & Pull Request Guidelines

No Git history is available in this checkout, so no repository-specific convention can be inferred. Use short, imperative commit subjects, for example `Add validation for inserted records`. Keep commits focused. Pull requests should explain the behavior change, list verification commands and outcomes, link relevant issues, and call out any data-format or configuration changes. Include request/response examples when modifying an MCP tool interface.

## Security & Configuration

Do not commit real database contents, secrets, or environment-specific configuration. Validate untrusted tool input before using it in future storage or query features, and preserve backward compatibility for existing TinyDB documents.
