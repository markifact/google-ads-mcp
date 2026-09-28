# Google Ads MCP server: install instructions for AI agents

This is a hosted (remote) MCP server. There is nothing to download or run locally. Human-readable setup guide: https://www.markifact.com/google-ads-mcp

1. Add an HTTP MCP server with URL `https://api.markifact.com/mcp/google-ads`.
2. The server uses OAuth 2.1 with PKCE and dynamic client registration. Open the login URL the client shows and sign in with a Markifact account (free to create at https://www.markifact.com).
3. Once connected, call `find_operations` with a query such as "Google Ads report" to discover operation ids, `get_operation_inputs` for the schema, then `run_operation` for reads or `run_write_operation` for approval-gated writes.
4. Google Ads itself is authorized inside Markifact at https://www.markifact.com/app/connections, not in the client.

Do not ask the user for API keys or tokens. The server never accepts them in the chat.
