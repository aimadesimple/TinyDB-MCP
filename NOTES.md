---
name: notes-creating-tinydb-remote-mcp
description: Notes on creating a TinyDB Remote MCP server
author: Saurabh Lal
---

This `NOTES.md` file contains rough notes on this project. Other documents like `SETUP.md`, `GUIDE.md`, and `README.md` files can be created with the help of `NOTES.md`.

# Steps to create the TinyDB Remote MCP server

## Step 1: Initialise the Project

Initialise the project using uv as package manager

```bash
uv init --bare
```

Use `--bare` to create the smallest possible project, avoiding extra starter files like source code or a README. We do not use `--script` because it manages dependencies within a single Python file and uses a cached virtual environment instead of creating a project-specific virtual environment with `pyproject.toml` and `uv.lock`. The `--bare` flag gives us a minimal project while still benefiting from `uv`'s dependency management and local virtual environment.

## Step 2: Add Dependencies

Add the dependencies

```bash
uv add mcp tinydb
```

`uv` will automatically create a virtual environment for you when you run `uv add`.

Sometime you might have to activate the virtual environment using `venv source/bin/activate`

In the IDE, you might have to select the environment manually.

If I have to add dependencies later then use `uv sync`


## Step 3: Create the `server.py` file

Create the main MCP server file <file_name.py> tinydb_remote_server.py

```bash
touch tinydb_remote_server.py
```

## Step 4: Add the MCP boilerplate code

```python
// MCP BoilerPlate Code

from mcp.server.fastmcp import FastMCP

mcp = FastMCP('tinydb-remote-mcp')

@mcp.too()
def say_hello(name:str) -> str:
    """
    A simple MCP tool to print 'Hello <name>!', consistent greeting.
    Returns the string 'Hello <name>!'

    Args:
        name(str): Name of the user.

    Returns:
        str: The string "Hello <name>!"
    """
    return f'Hello {name}!'

if __name__ == "__main__":
    mcp.run(transport='sreammable-http')
```

## Step 6: Publish the Repository to GitHub

Use the GitHub CLI (`gh`) to authenticate, create the public repository, and
open it in a browser after the local Git repository has been initialized and
the initial commit has been created.

```bash
gh auth login --hostname github.com --web --git-protocol https
gh auth status
gh repo create AIMadeSimple/TinyDB-MCP --public --source=. --remote=origin --push
gh repo view AIMadeSimple/TinyDB-MCP --web
```

The `gh repo create` command creates the repository under the `AIMadeSimple`
organization, configures it as the local `origin` remote, and pushes the
current branch. Run it from the project root after committing the intended
source files.
