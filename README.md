# Saaskly MCP server

Independent UK reviews, rankings and real pricing for business software, available to AI agents as a public MCP server and REST API.

[Saaskly](https://saaskly.com) reviews business software first-hand or researches it thoroughly, scores it out of 5, and publishes the month-to-month price next to the annual-commit price so buyers see the real cost. Everything published on the site is readable by agents through the interfaces below. No account, no API key, no cost.

This repository holds the public manifests for the hosted server (`server.json` for the MCP Registry, `.mcp.json` and `mcp.json` for plugin directories). The server itself runs at `https://saaskly.com/mcp` and its source lives in the site's private codebase.

## Connect

**Endpoint:** `https://saaskly.com/mcp` (Streamable HTTP, stateless, JSON responses, no authentication)

Claude Code:

```bash
claude mcp add --transport http saaskly https://saaskly.com/mcp
```

Claude Desktop, Cursor, Windsurf and any client that reads an `mcpServers` block:

```json
{
  "mcpServers": {
    "saaskly": {
      "type": "http",
      "url": "https://saaskly.com/mcp"
    }
  }
}
```

Raw JSON-RPC:

```bash
curl -s -X POST https://saaskly.com/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

Server card: [saaskly.com/.well-known/mcp/server-card.json](https://saaskly.com/.well-known/mcp/server-card.json)

## Tools (all read-only)

| Tool | What it returns |
|---|---|
| `list_categories` | The software categories Saaskly covers, with slugs and how many providers are ranked in each |
| `list_providers` | Ranked providers in a category with editorial score (0 to 5), one-line summary, entry price and partner disclosure |
| `get_provider` | The full review of one provider as Markdown: verdict, pros and cons, month-to-month and annual pricing, spec sheet, disclosure |
| `compare_providers` | A side-by-side comparison table (Markdown) for a category, optionally limited to given provider slugs |
| `search` | Providers and articles matching a keyword, with links and Markdown mirrors |
| `list_articles` | Published comparison articles and buying guides, optionally filtered by category |
| `get_article` | One article as Markdown |
| `get_page` | A policy or about page as Markdown: editorial policy (scoring method), AI transparency, about, advertise, terms, privacy, cookies, contact |

## Prompts

Ready-made workflows over the tools, listed by `prompts/list` and fetched with `prompts/get`.

- `recommend_provider` (`category`, `need`): ranks the category, reads the top reviews and explains the pick with both price bases, the renewal premium where the category has one, and disclosures.
- `compare_two` (`category`, `providerA`, `providerB`): summarises the spec-sheet differences and both editorial verdicts, with source URLs.

## Resources

- `https://saaskly.com/llms.txt`: site index
- `https://saaskly.com/category/{category}.md`: category ranking and comparison table
- `https://saaskly.com/category/{category}/{slug}.md`: one provider review

Every public page on saaskly.com also has a Markdown mirror: append `.md` to the URL.

## Categories covered

VoIP and phone systems, transactional email, cloud management platforms, SEO/GEO/AEO tools, social media management, content creation, domain names, VPNs, CRM, VPS and cloud servers, GPU and high performance compute, password managers, accounting software, antivirus and endpoint security, CMS and website builders. Scores are editorial opinion; partner relationships are disclosed on every page and never affect scores or rank.

## REST API

Base URL `https://saaskly.com/api/v1`. OpenAPI 3.1 at [saaskly.com/openapi.json](https://saaskly.com/openapi.json). Full documentation, query grammar, batch reads and the deprecation policy: [saaskly.com/developers](https://saaskly.com/developers).

## Rate limits

300 requests per 60 seconds per client IP across `/api/*` and `/mcp`, with IETF `RateLimit-*` headers on every response and a JSON 429 with `Retry-After` over the limit. Please identify your agent in the `User-Agent` header with a contact URL.

## Directory listings

[Smithery](https://smithery.ai/servers/tim-hsu1/saaskly) · [mcp.so](https://mcp.so/servers/saaskly) · [mcpservers.org](https://mcpservers.org/servers/saasklycom-mcp) · [Glama](https://glama.ai/mcp/connectors/com.saaskly/saaskly)

## Contact and corrections

Vendors can suggest corrections through the [vendor portal](https://saaskly.com/portal/login) after verifying a company-domain email address. Everyone else: [saaskly.com/contact](https://saaskly.com/contact).

Saaskly is a trading name of Next Generation ICT Ltd. The manifests in this repository are MIT licensed; the reviews and data served by the API remain the property of Next Generation ICT Ltd and are published for reading and quoting with attribution.
