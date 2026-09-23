# MCP Resources as Tools Demo

This example shows how to expose MCP resources through generated tools, so clients that only understand the tool interface can still discover and read resource-backed data.

It uses FastMCP's `ResourcesAsTools` transform to convert resource metadata and resource reads into callable tools such as:

- `list_resources`
- `read_resource`

## What this example demonstrates

The server defines several resource endpoints:

- Static resource: `config://app`
- Static resource: `readme://main`
- Templated resource: `user://{user_id}/profile`
- Templated resource: `file://{directory}/{filename}`

When the transform is added, FastMCP automatically exposes the resource listing and reading flow as tools, which makes it easier to work with clients that are tool-only.

## Files

- `server.py` — defines the MCP server and resource endpoints
- `client.py` — connects to the server and calls the generated tools

## Run the server

From the project root:

```bash
python resources_as_tools/server.py
```

Or, if you are using `uv`:

```bash
uv run python resources_as_tools/server.py
```

## Run the client

From the project root:

```bash
python resources_as_tools/client.py
```

Or:

```bash
uv run python resources_as_tools/client.py
```

## What the client does

The client:

1. Connects to the server using `fastmcp.Client`
2. Lists available tools
3. Calls `list_resources` to inspect resource metadata
4. Calls `read_resource` for a static resource such as `config://app`
5. Calls `read_resource` for a template-backed resource such as `user://42/profile`

## Example behavior

The server exposes resource data like configuration JSON and user profile JSON, and the client prints those values as normal tool results.

Example outputs include:

- app config JSON
- project README text
- JSON profile data for a specific user ID

## Why this pattern is useful

This is helpful when:

- a client only supports tool calls
- you want to bridge a resource-based API into a tool-based workflow
- you want a simple way to surface discovery and read operations through a uniform tool interface

## Notes

This demo is intentionally small and focused. It is designed to show the core idea without extra application complexity.
