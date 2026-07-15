# TinyDB Remote MCP Server

A small, practical example of a remote [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) server backed by [TinyDB](https://tinydb.readthedocs.io/). It is intended to help people understand how to build an MCP server, deploy it, and connect it to an AI client.

The complete build notes—including project setup, GitHub publishing, Render deployment, and MCP Inspector usage—are in [NOTES.md](NOTES.md).

## What it does

The server exposes three Streamable HTTP MCP tools:

| Tool | Description |
| --- | --- |
| `insert_data` | Stores one JSON-compatible document in TinyDB. |
| `get_all_data` | Returns every stored document. |
| `delete_all_data` | Permanently removes every stored document. |

For example, `insert_data` accepts:

```json
{
  "name": "John Doe",
  "age": 30
}
```

Data is stored locally in `db.json`. This file is intentionally ignored by Git.

## Requirements

- Python 3.13 or newer
- [uv](https://docs.astral.sh/uv/)

## Run locally

Install the locked dependencies and start the server:

```bash
uv sync
uv run --frozen python tinydb_remote_server.py
```

The server listens on `0.0.0.0:10000` by default. Set `PORT` to use a different port:

```bash
PORT=8000 uv run --frozen python tinydb_remote_server.py
```

Its MCP endpoint is available at:

```text
http://localhost:10000/mcp
```

## Deploy to Render

Create a Python Web Service from this repository's `main` branch and use:

```bash
# Build command
uv sync --frozen && uv cache prune --ci

# Start command
uv run --frozen python tinydb_remote_server.py
```

Render supplies `PORT`; do not configure it manually. Once deployed, the endpoint is:

```text
https://<render-service-name>.onrender.com/mcp
```

## Use the MCP

### MCP Inspector

Start the Inspector:

```bash
npx @modelcontextprotocol/inspector
```

Open the displayed local URL, choose **Streamable HTTP**, and enter either the local or deployed `/mcp` endpoint. To list tools from the command line:

```bash
npx @modelcontextprotocol/inspector --cli https://<render-service-name>.onrender.com/mcp --transport http --method tools/list
```

### OpenAI web app

In ChatGPT, enable Developer mode, then create an app from **Settings > Plugins** (or `https://chatgpt.com/plugins`). Give it a name and description, and set its MCP server URL to:

```text
https://<render-service-name>.onrender.com/mcp
```

After creating it, select the app in a ChatGPT project and ask ChatGPT to insert, retrieve, or clear test data.

### Codex, Claude web app, and Claude Code

Add this server to the MCP configuration for your client, using its Streamable HTTP URL:

```text
https://<render-service-name>.onrender.com/mcp
```

The client-specific configuration screens and file formats differ, but each needs the same endpoint and should show `insert_data`, `get_all_data`, and `delete_all_data` after connecting.

## Important notes

- This server is public and unauthenticated. Use test data only; never store sensitive information.
- TinyDB writes to the host filesystem. On Render's Free tier, `db.json` is ephemeral and may be lost after a restart or redeploy.
- `delete_all_data` permanently clears all stored records.

## Development

Check the server for Python syntax errors:

```bash
uv run --frozen python -m py_compile tinydb_remote_server.py
```

When adding dependencies, use `uv add <package>` so `pyproject.toml` and `uv.lock` stay synchronized.

## FAQ

### Should I create a virtual environment before running `uv init`?

For a new Python project, run `uv init` first. Then add dependencies with `uv add`; uv creates and manages the project environment for you.

```bash
mkdir my-project
cd my-project
uv init
uv add requests
```
