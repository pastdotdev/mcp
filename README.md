# past.dev MCP servers

[past.dev](https://past.dev) is memory for AI agents. Two endpoints, `/ingest` and `/recall`:
an agent writes what happened, then asks a question and gets back what is true now, with dated
sources it can cite.

This repository holds the [Model Context Protocol](https://modelcontextprotocol.io) manifest for
past.dev's two MCP servers. Both are **remote**: there is nothing to install, no package to pull,
no container to run. Point a client at a URL.

| Server | URL | Authentication | What it does |
|---|---|---|---|
| Docs | `https://past.dev/mcp` | none | Search and read the Memory API documentation |
| Account | `https://app.past.dev/mcp` | OAuth | Work with an organization, its projects, keys, members, access rules, usage and memory |

## Add it to your client

### Claude Code

```bash
claude mcp add --transport http past-docs https://past.dev/mcp
claude mcp add --transport http past https://app.past.dev/mcp
```

The account server opens a browser for OAuth on first use. No API key to paste.

### Cursor, VS Code, Windsurf, Zed, Cline, Continue

```json
{
  "mcpServers": {
    "past-docs": { "type": "http", "url": "https://past.dev/mcp" },
    "past": { "type": "http", "url": "https://app.past.dev/mcp" }
  }
}
```

Clients that do not speak Streamable HTTP natively can bridge through
[`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "past": { "command": "npx", "args": ["-y", "mcp-remote", "https://app.past.dev/mcp"] }
  }
}
```

## The docs server

No account, no key, no rate limit worth mentioning. It is there so an agent can read the Memory
API before writing a line against it.

| Tool | What it returns |
|---|---|
| `search_docs` | Matching documentation pages with an excerpt, over the full corpus |
| `read_page` | One page in full |
| `list_pages` | Every page available |

Verified against the live server on 29 September 2026.

## The account server

OAuth, and the tools a caller sees are filtered by their role in the organization. It covers
projects, API keys, members, access rules, audiences, usage and the request log, and it reaches
memory itself: `recall`, `remember` and `answer`, plus `get_timeline`, `get_lineage` and
`explain_ingestion` for looking at how a fact came to be.

The full, current list is in [`server.json`](server.json) and at
[`/.well-known/mcp.json`](https://past.dev/.well-known/mcp.json), which is generated from the
server rather than written by hand.

## The manifest

[`server.json`](server.json) follows the
[MCP Registry schema](https://static.modelcontextprotocol.io/schemas/2025-12-11/server.schema.json).
The name `dev.past/past` is the reverse-DNS form of `past.dev`, which is what the registry
requires for domain-based authentication.

## Links

- Memory API documentation: [past.dev/docs/memory-api/overview](https://past.dev/docs/memory-api/overview)
- MCP documentation: [past.dev/docs/mcp/overview](https://past.dev/docs/mcp/overview)
- OpenAPI: [past.dev/openapi.json](https://past.dev/openapi.json)
- Benchmarks, with the harnesses published: [past.dev/benchmarks](https://past.dev/benchmarks) and [pastdotdev/benchmarks](https://github.com/pastdotdev/benchmarks)
- Sign up: [sso.past.dev/sign-up](https://sso.past.dev/sign-up)
- Community: [past.dev/slack](https://past.dev/slack)

## License

MIT. See [LICENSE](LICENSE).
