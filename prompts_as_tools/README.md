# MCP Prompts as Tools Demo

This example shows how to expose MCP prompts through generated tools so clients that only support tool calls can still discover and use prompt templates.

It uses FastMCP's `PromptsAsTools` transform to turn prompt metadata and prompt retrieval into callable tools such as:

- `list_prompts`
- `get_prompt`

## What this example demonstrates

The server defines several prompt templates:

- `explain_concept(concept: str)`
- `analyze_code(code: str, language: str = "python", focus: str = "all")`
- `review_pull_request(title: str, description: str, diff: str, guidelines: str = "")`

When the transform is added, FastMCP automatically exposes these prompts as tools for clients that are tool-only.

## Files

- `server.py` — defines the MCP server and prompt templates
- `client.py` — connects to the server and calls the generated tool interface

## Run the server

From the project root:

```bash
python prompts_as_tools/server.py
```

Or, if you are using `uv`:

```bash
uv run python prompts_as_tools/server.py
```

## Run the client

From the project root:

```bash
python prompts_as_tools/client.py
```

Or:

```bash
uv run python prompts_as_tools/client.py
```

## What the client does

The client:

1. Connects to the MCP server with `fastmcp.Client`
2. Lists available tools
3. Calls `list_prompts` to inspect the available prompt definitions
4. Calls `get_prompt` for a simple prompt, such as explaining a concept
5. Calls `get_prompt` for a more complex prompt with optional arguments, such as code analysis

## Example behavior

The server returns prompt metadata including argument names, required/optional status, and descriptions. The client then fetches generated prompt messages, which are returned as structured JSON with content and role information.

## Why this pattern is useful

This is useful when:

- a client only supports tool calls
- you want to expose prompt templates through a uniform tool interface
- you want prompt discovery and retrieval without requiring direct prompt protocol support

## Notes

This is a compact demonstration focused on the core idea: converting MCP prompts into tools while keeping the underlying prompt logic intact.
