## Run the server 
```bash
fastmcp run my_server.py:mcp
or
uv run .\   my_server.py mcp
### http
fastmcp run my_server.py:mcp --transport http --port 8000
```

## Run the server with prefabricated UI 
```bash
fastmcp dev apps .\my_server_with_ui.py
```


## Inspect
```bash
npx @modelcontextprotocol/inspector python my_server.py
```

## Run the client 

```bash
python .\02_client_server\my_client.py
```

