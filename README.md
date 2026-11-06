# getpeppr MCP server

A hosted [Model Context Protocol](https://modelcontextprotocol.io) server for **Peppol e-invoicing**, run by [getpeppr](https://getpeppr.dev).

Your assistant can plan a getpeppr integration, search the getpeppr documentation, check a test invoice against the Peppol rules and look up a participant on the Peppol network. With a free getpeppr sandbox account, it can also send test invoices and follow their status.

> **This repository holds no source code.** The server is hosted and operated by getpeppr (Zero Loop Labs Ltd, UK). This repository describes it for MCP registries and directories. Nothing to install: you add an address to your assistant.
>
> Full documentation: **https://getpeppr.dev/docs/mcp/**

> **Sandbox only.** The server never sends a real invoice and never acts in production. Test invoices go to the sandbox test receiver `9925:BE0314595348`, and a production key (`sk_live_…`) is refused: keep it out of your assistant's configuration. The participant lookup, on the other hand, reads the live Peppol network.

## Addresses

Registry name: `dev.getpeppr/mcp` · transport: streamable HTTP.

| Address | When to use it |
| --- | --- |
| `https://api.getpeppr.dev/mcp` | The four public tools work at once, without an account. The account tools ask you to sign in the first time you use one. For clients that open a sign-in page when a tool asks, such as Claude Code and Codex. |
| `https://api.getpeppr.dev/mcp/auth` | Asks you to sign in to getpeppr when you connect. For clients that sign in at connection time, such as VS Code, Cursor and Claude (web & desktop). |

## Configure your assistant

### VS Code (GitHub Copilot)

Add to `.vscode/mcp.json` in your workspace, or to your user `mcp.json`:

```json
{
  "servers": {
    "getpeppr": {
      "type": "http",
      "url": "https://api.getpeppr.dev/mcp/auth"
    }
  }
}
```

Or from a terminal:

```bash
code --add-mcp '{"name":"getpeppr","type":"http","url":"https://api.getpeppr.dev/mcp/auth"}'
```

VS Code asks you to sign in to getpeppr when it connects.

### Claude Code

```bash
claude mcp add --transport http getpeppr https://api.getpeppr.dev/mcp
```

When you first call an account tool, run `/mcp` in Claude Code and sign in to getpeppr.

### Codex

```bash
codex mcp add getpeppr --url https://api.getpeppr.dev/mcp
codex mcp login getpeppr
```

Run the login before using the account tools.

### Cursor and Claude (web & desktop)

Both connect through `https://api.getpeppr.dev/mcp/auth`. getpeppr is also listed in Claude's connector directory. See the [documentation](https://getpeppr.dev/docs/mcp/) for the one-click Cursor install.

### Signing in

Signing in uses your getpeppr account; there is no API key to copy. You choose the organisation the assistant acts for. You can also pass a sandbox API key (`sk_sandbox_…`) as a bearer token instead of signing in.

No account yet? [Create a free sandbox account](https://console.getpeppr.dev/sign-up?source=mcp).

## Tools

| Tool | Account | What it does |
| --- | --- | --- |
| `lookup_participant` | No | Whether an identifier is registered on the production Peppol network, with which provider, and what the directory says about it. |
| `validate_invoice` | No | Checks a UBL document against the official Peppol rulebooks, or a JSON invoice against the getpeppr SDK's rules. Give it sample or test invoices only, never a real invoice or real people's data. |
| `plan_integration` | No | The steps and code for your case (your own company, or a platform sending for customers) and your countries. |
| `search_docs` | No | Finds getpeppr documentation pages, each with a short excerpt. |
| `send_test_invoice` | Yes, writes (sandbox) | Sends a sandbox invoice to the sandbox test receiver `9925:BE0314595348`. |
| `get_document_status` | Yes, reads | The status of one of your documents and its event timeline. |
| `create_test_customer` | Yes, writes (sandbox) | Platform accounts: creates a sandbox customer company to send as. |

## From zero to a delivered test invoice

1. `plan_integration` gives the steps and code for your case; `search_docs` finds the pages to read.
2. `validate_invoice` and `lookup_participant` work without an account.
3. Create a free getpeppr account and register your own company in the console.
4. `send_test_invoice` sends to the sandbox test receiver. Platform accounts first call `create_test_customer`, then send as that customer.
5. `get_document_status` follows the document until it is delivered or failed.

Going live (an `sk_live_` key, real receivers, webhooks) happens in the getpeppr console and in your own code, never through this server.

## Limits

Public tools are rate limited per network address, and account tools per account. getpeppr can turn the MCP tools off without notice to protect the service; they then answer `mcp.temporarily_disabled`, and the API itself is not affected. Details: [Limits](https://getpeppr.dev/docs/mcp/).

## Privacy and contact

- Privacy policy: https://getpeppr.dev/privacy/
- Questions: hello@getpeppr.dev
