<p align="center"><img src="https://raw.githubusercontent.com/send21io/send21io/main/assets/social-1280x640.png" alt="send21" width="640"></p>

# send21 MCP

Remote [Model Context Protocol](https://modelcontextprotocol.io/) server for **send21**: crypto pay links and API for people and AI agents.

send21 prepares payment instructions for BTC, USDC, USDT, and EURC. The payer signs in their own wallet. send21 never holds keys, never signs, and never broadcasts.

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
- **OAuth sign-in:** clients that support MCP sign-in (Claude, Claude Code, Cursor, VS Code, Muse Code) can use `https://send21.io/mcp/oauth` with no headers. You sign in to send21 in the browser and approve the app; connected apps only ever use receiving addresses the owner approved. Details: https://send21.io/docs/mcp ("Sign in instead of a key").
- Scopes such as `drafts:write`, `drafts:read`, `webhooks:manage` **cannot move funds**.
- `tools/list` works without authentication.

## What the tools do

14 tools (see live `tools/list`): create_draft, get_draft, get_draft_status, list_draft_quotes, get_transaction_template, submit_lightning_proof, create_payment_request, get_payment_request, list_address_book, propose_receiving_address, get_account_status, create_account, get_rate, get_fee_schedule. Signing and broadcast stay in the user’s wallet.

**Agents onboard themselves; humans approve where the money lands.** An agent proposes a receiving address (`propose_receiving_address`) and the owner approves it after signing in, with a passkey or an authenticator code; no API key can approve an address, and any other address is refused (`422 address_not_approved`). An agent without a key can sign its owner up (`create_account`), but it cannot create anything until the owner takes the account over from an emailed link (`403 owner_not_confirmed` until then). Details: https://send21.io/docs/mcp

## Brand

send21: crypto pay links and API for people and AI agents.  
Contact: info@send21.io

## License

MIT
