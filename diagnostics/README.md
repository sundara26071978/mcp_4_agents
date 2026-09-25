# Diagnostics MCP Server and Client

This example is a small observability-focused MCP setup for testing how a server and client behave when they encounter both successful and intentionally failing MCP components.

It demonstrates how to trace:

- tool calls
- resource reads
- prompt retrieval
- proxied components mounted from another server
- exceptions raised by deliberately failing operations

## What this example contains

### Server

The server in `server.py` creates a `FastMCP` instance named `Diagnostics Server` and registers a mix of normal and error-producing elements:

- Successful tool: `ping`
- Successful resource: `diag://status`
- Successful templated resource: `diag://echo/{message}`
- Successful prompt: `greet`
- Failing tool: `fail_tool`
- Failing resource: `diag://error`
- Failing templated resource: `diag://error/{code}`
- Failing prompt: `fail`

It also mounts a proxied echo server namespace named `proxied`, so the client can test both local and proxied MCP components.

### Client

The client in `client_with_tracing.py` connects to the server over SSE and:

1. Lists tools, resources, and prompts
2. Calls successful components
3. Calls intentionally failing components to confirm errors are surfaced
4. Exports traces to OpenTelemetry-compatible tooling

This makes it useful for inspecting request flow and understanding how MCP errors appear in a tracing pipeline.

## Files

- `server.py` — diagnostics MCP server with success and failure cases
- `client_with_tracing.py` — client that exercises the server and emits tracing data

## Prerequisites

You will need:

- Python 3.10+
- `uv` installed
- An OpenTelemetry trace collector or viewer such as `otel-desktop-viewer`

## Run the example

Start the trace viewer in one terminal:

```bash
otel-desktop-viewer
```

Then start the diagnostics server with tracing enabled in another terminal:

```bash
uv run python diagnostics/server.py
```

If your setup uses a tracing wrapper for the MCP server entrypoint, the equivalent pattern is:

```bash
uv run python run_with_tracing.py diagnostics/server.py --transport sse --port 8001
```

Finally, run the client:

```bash
uv run python diagnostics/client_with_tracing.py
```

The client defaults to connecting to:

```text
http://localhost:8001/sse
```

## What to expect

The client prints a structured walkthrough of:

- successful tool/resource/prompt calls
- proxied tool/resource/prompt calls
- expected failures for intentionally broken MCP entries

You can then inspect the trace viewer to see spans for each step and confirm how exceptions are reported through the tracing stack.

## Why it is useful

This example is helpful when you want to:

- validate that MCP servers are reporting normal and exceptional behavior correctly
- observe spans across tools, resources, and prompts
- debug tracing setup for FastMCP applications
- confirm that mounted or proxied services behave correctly in an instrumented environment

## Notes

This demo is intentionally designed for diagnostics, not for production logic. The failing components are expected to raise exceptions so you can observe them in the client output and in the tracing dashboard.
