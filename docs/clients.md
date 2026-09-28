# Connect the Google Ads MCP server to your client

All clients use the same endpoint and the same browser login. Setup guide with screenshots: https://www.markifact.com/google-ads-mcp

Endpoint: `https://api.markifact.com/mcp/google-ads`

## Claude web and desktop

1. Settings → Connectors → **Add custom connector**.
2. Name `Google Ads`, URL `https://api.markifact.com/mcp/google-ads`, **Add**.
3. Sign in with your Markifact account when the browser opens.

## Claude Code

```bash
claude mcp add --transport http google-ads https://api.markifact.com/mcp/google-ads
```

Or install it as a plugin, which also adds the marketing skills and slash commands:

```bash
claude plugin marketplace add markifact/markifact-mcp
/plugin install google-ads@markifact
```

## ChatGPT and Codex

Install the [Markifact plugin](https://chatgpt.com/plugins/plugin_asdk_app_69e795096e888191b3908a4dd48a323d) from the ChatGPT Plugin Directory and call it with `@Markifact`. Codex uses the same plugin. For Codex CLI, add to `~/.codex/config.toml`:

```toml
[mcp_servers.google_ads]
url = "https://api.markifact.com/mcp/google-ads"
```

## Cursor

Settings → MCP → Add server, or merge this into `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "google-ads": { "url": "https://api.markifact.com/mcp/google-ads" }
  }
}
```

## Gemini CLI

```bash
gemini extensions install github.com/markifact/google-ads-mcp
```

## Windsurf and other MCP clients

Streamable HTTP at `https://api.markifact.com/mcp/google-ads`. OAuth 2.1 with PKCE and dynamic client registration. Discovery metadata is at `https://api.markifact.com/.well-known/oauth-protected-resource/mcp/google-ads`.

## After connecting

Connect Google Ads on the [Connections page](https://www.markifact.com/app/connections) if you have not already. Then run a read-only report first and check the account, currency and date range before you allow any change.
