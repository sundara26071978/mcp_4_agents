# FastMCP config examples

This folder demonstrates how to configure a FastMCP server with a declarative `fastmcp.json` file instead of passing all settings on the command line.

The examples use the same `server.py` entrypoint and show several common patterns:

- minimal config
- explicit source + runtime settings
- HTTP deployment configuration
- environment-variable interpolation

## Files

- `server.py` – example FastMCP server exposing a few tools and resources
- `simple.fastmcp.json` – smallest valid config using stdio transport
- `fastmcp.json` – a minimal file with `source`, `environment`, and `deployment`
- `full_example.fastmcp.json` – more complete example with host, port, env vars, and dependencies
- `env_interpolation_example.json` – example of variable expansion in deployment env values

## Run an example

From this directory:

```bash
fastmcp run simple.fastmcp.json
```

or:

```bash
fastmcp run fastmcp.json
```

You can also inspect or install a config-based server:

```bash
fastmcp inspect simple.fastmcp.json
fastmcp install simple.fastmcp.json
```

## Example configuration shape

```json
{
  "$schema": "https://gofastmcp.com/schemas/fastmcp/v1.json",
  "source": {
    "path": "server.py"
  },
  "environment": {
    "python": "3.12",
    "dependencies": ["requests"]
  },
  "deployment": {
    "transport": "stdio"
  }
}
```

## Notes

- `source.path` points to the Python file or module that contains the FastMCP app.
- `environment.dependencies` declares packages that should be available in the server environment.
- `deployment.transport` controls how the server is exposed (`stdio`, `http`, etc.).
- Environment variables can be interpolated using `${VAR_NAME}` syntax in deployment settings.

These examples are useful as a starting point for packaging FastMCP servers for CLI use, local development, or deployment into MCP clients.
