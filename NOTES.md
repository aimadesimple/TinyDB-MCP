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

## Step 5: Add the tools, resources and prompts to the MCP

In this step you add the tools, resources and prompts required to accomplish the goal.


## Step 6: Publish the Repository to GitHub

Use Git to initialize and commit the project, then use the GitHub CLI (`gh`)
to authenticate, create the public repository, push the initial commit, and
verify the result. Run these commands from the project root.

```bash
# Initialize the local repository and create the initial commit
git init -b main
git add AGENTS.md NOTES.md .gitignore pyproject.toml tinydb_remote_server.py uv.lock
git status --short
git diff --cached --check
git commit -m "Initial TinyDB MCP server"

# Authenticate GitHub CLI in a browser and confirm the active account
gh auth login --hostname github.com --web --git-protocol https
gh auth status

# Create the public repository, configure origin, and push main
gh repo create AIMadeSimple/TinyDB-MCP --public --source=. --remote=origin --push

# Confirm the repository settings and local remote
gh repo view AIMadeSimple/TinyDB-MCP --json nameWithOwner,visibility,defaultBranchRef,url
git remote -v
```

For later changes, stage the intended files, create a focused commit, and push
the current `main` branch to GitHub:

```bash
git add NOTES.md
git commit -m "Update publishing notes"
git push -u origin main
```

The `gh repo create` command creates the repository under the `AIMadeSimple`
organization, configures it as the local `origin` remote, and pushes the
current `main` branch. The explicit `git add` list avoids committing local
environment files or the runtime TinyDB database.
