<p align="center"><img src="https://raw.githubusercontent.com/send21io/send21io/main/assets/social-1280x640.png" alt="send21" width="640"></p>

# send21 MCP

Remote [Model Context Protocol](https://modelcontextprotocol.io/) server for **send21**: crypto pay links and API for people and AI agents.

send21 prepares payment instructions for BTC, USDC, USDT, and EURC. The payer signs in their own wallet. send21 never holds keys, never signs, and never broadcasts.

**Live endpoint (Streamable HTTP):** https://send21.io/mcp  
**Descriptor:** https://send21.io/.well-known/mcp.json  
**Agent docs:** https://send21.io/llms.txt  
**OpenAPI:** https://send21.io/swagger/v1/swagger.json  
**Site:** https://send21.io

## Connect

Point an MCP client at the remote server:

```json
{
  "mcpServers": {
    "send21": {
      "url": "https://send21.io/mcp",
      "transport": "streamable-http",
      "headers": {
        "Authorization": "Bearer s21_YOUR_API_KEY"
      }
    }
  }
}
```

- Create scoped API keys at https://send21.io/api-keys (`s21_…`).
- Scopes such as `drafts:write`, `drafts:read`, `webhooks:manage` **cannot move funds**.
- `tools/list` works without authentication.

## What the tools do

Typical tools (see live `tools/list`): create drafts / payment requests, fetch transaction templates, rates, and fee schedule. Signing and broadcast stay in the user’s wallet.

## Brand

send21: crypto pay links and API for people and AI agents.  
Contact: integration@send21.io

## License

MIT
