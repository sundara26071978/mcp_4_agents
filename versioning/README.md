# MCP Component Versioning

This folder contains FastMCP examples for registering multiple versions of tools, resources, and prompts, selecting versions from a client, and exposing filtered API surfaces.

These are self-contained, in-process demos: each script creates a `FastMCP` server and connects a `Client` directly to it. They do not require separate server and client terminals.

## Examples

### `versioned_components.py` — versioned tools, resources, and prompts

Registers two versions of the `calculate` tool, the `config://app` resource, and the `summarize` prompt. The client lists components, inspects the version metadata, and calls the default or a requested tool version. The default is the highest available version.

### `client_version_selection.py` — discover and request versions

Registers three versions of the `charge` tool. The client reads the available versions from component metadata, calls versions `1.0`, `1.1`, and `2.0`, then requests a nonexistent version to demonstrate handling the resulting `ToolError`.

### `version_filters.py` — create separate API surfaces

Defines a shared provider with versions `1.0`, `2.0`, and `3.0` of the `process` tool, plus an unversioned `health` tool. `VersionFilter` then creates separate API surfaces for each version range. The client lists each surface and compares how the same `process` call behaves across them.

## Requirements

The project uses Python 3.13 or newer and declares `fastmcp` and `rich` as dependencies in the root `pyproject.toml`. With `uv` installed, run the examples from the repository root; `uv run` uses the project's dependencies.

## Run

Run any example independently from the repository root:

```bash
uv run python versioning/versioned_components.py
```

```bash
uv run python versioning/client_version_selection.py
```

```bash
uv run python versioning/version_filters.py
```

Each script prints its results to the terminal. The examples use Rich formatting for readable output.

## Key ideas

- Versions are attached to MCP components through the `version` argument on registration.
- Clients can inspect component metadata to discover the current and available versions.
- A client can request a particular version when calling a component; requesting an unavailable tool version raises a `ToolError` in the client-selection example.
- `VersionFilter` can expose different version ranges as separate server API surfaces, while unversioned components pass through the filters.
