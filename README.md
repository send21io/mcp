<p align="center"><img src="https://raw.githubusercontent.com/send21io/send21io/main/assets/social-1280x640.png" alt="send21" width="640"></p>

# send21 MCP

Remote [Model Context Protocol](https://modelcontextprotocol.io/) server for **send21**: crypto pay links and API for people and AI agents.

send21 prepares payment instructions for BTC (on-chain and Lightning), USDC, USDT, EURC, SOL and ETH. The payer signs in their own wallet. send21 never holds keys, never signs, and never broadcasts.

**Live endpoint (Streamable HTTP):** https://send21.io/mcp (API key), https://send21.io/mcp/oauth (OAuth sign-in)  
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

Auth: API key or OAuth sign-in.

- **API key:** create scoped keys at https://send21.io/api-keys (`s21_…`) and send them as above.
- **OAuth sign-in:** use `https://send21.io/mcp/oauth` with no headers; see [Sign in instead of an API key](#sign-in-instead-of-an-api-key) below.
- Scopes such as `drafts:write`, `drafts:read`, `webhooks:manage` **cannot move funds**.
- `tools/list` and `get_fee_schedule` work without authentication.

## Sign in instead of an API key

Clients that support MCP sign-in (OAuth) can connect to `https://send21.io/mcp/oauth` with no key and no headers. The client opens send21 in your browser; you sign in, see what the app asks for, and approve. A connected app only ever uses receiving addresses you approved. It is listed under [API keys](https://send21.io/api-keys) as a connected app, where Disconnect ends it at once, and a connection nobody uses for 90 days ends by itself.

**Claude Code**

```bash
claude mcp add --transport http send21 https://send21.io/mcp/oauth
# then run /mcp in Claude Code and choose send21 to sign in
# (or, from your shell: claude mcp login send21)
```

**Cursor** (`.cursor/mcp.json` in your project, or `~/.cursor/mcp.json` for all projects)

```json
{
  "mcpServers": {
    "send21": {
      "url": "https://send21.io/mcp/oauth"
    }
  }
}
```

**VS Code** (`.vscode/mcp.json`)

```json
{
  "servers": {
    "send21": {
      "type": "http",
      "url": "https://send21.io/mcp/oauth"
    }
  }
}
```

**Any MCP client that supports OAuth:** add `https://send21.io/mcp/oauth` as a remote (Streamable HTTP) MCP server with no headers, then sign in when the client asks.

Prefer an API key? It still works: use `https://send21.io/mcp` with `Authorization: Bearer s21_...`, a scoped key created at https://send21.io/api-keys. `tools/list` and `get_fee_schedule` work without either. Details: https://send21.io/docs/mcp

## What the tools do

14 tools (see live `tools/list`): create_draft, get_draft, get_draft_status, list_draft_quotes, get_transaction_template, submit_lightning_proof, create_payment_request, get_payment_request, list_address_book, propose_receiving_address, get_account_status, create_account, get_rate, get_fee_schedule. Signing and broadcast stay in the user’s wallet.

**Agents onboard themselves; humans approve where the money lands.** An agent proposes a receiving address (`propose_receiving_address`) and the owner approves it after signing in, with a passkey or an authenticator code; no API key can approve an address, and any other address is refused (`422 address_not_approved`). An agent without a key can sign its owner up (`create_account`), but it cannot create anything until the owner takes the account over from an emailed link (`403 owner_not_confirmed` until then). Details: https://send21.io/docs/mcp

## Brand

send21: crypto pay links and API for people and AI agents.  
Contact: info@send21.io

## License

MIT
