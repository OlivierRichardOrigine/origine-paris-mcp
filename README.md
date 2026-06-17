# Origine Paris MCP server

Official Model Context Protocol (MCP) server of [Origine Paris](https://origineparis.com), the Parisian fine jewellery house crafting recycled 18ct gold jewellery set with IGI-certified lab-grown diamonds. Collections and bespoke creations (engagement rings, wedding bands, necklaces, bracelets, earrings) are presented by appointment at 21 rue de la Paix, Paris.

This repository documents the hosted server. The server is public and strictly read-only: it serves brand identity and catalogue data generated from public sources, namely the JSON-LD published on origineparis.com, the site llms.txt and Wikidata ([Q139905888](https://www.wikidata.org/wiki/Q139905888)). Anything absent from those sources is reported as unknown, never invented.

## Endpoint

| | |
| --- | --- |
| URL | `https://mcp.origineparis.com/mcp` |
| Transport | Streamable HTTP |
| Authentication | None required |
| Access | Public, read-only |

Health check: `https://mcp.origineparis.com/health`. Discovery manifest: `https://mcp.origineparis.com/.well-known/mcp/server.json`, mirrored in this repository as [server.json](server.json).

## Tools

| Tool | Description |
| --- | --- |
| `get_brand_identity` | Brand identity: trading name, legal identity (SIREN), positioning, by-appointment address, contacts and official profiles. |
| `get_founders` | The two founders, with their roles, Wikidata QIDs and short biographies. |
| `get_person_profile` | Detailed sourced profile of one founder (career, dates, references); pass `name` or `qid`. |
| `get_entity_graph` | The company and its founders as sourced nodes and edges. |
| `get_jsonld_graph` | The raw JSON-LD blocks from origineparis.com, served as published. |
| `get_llms_context` | Catalogue index and a short agent context, from the site llms.txt. |
| `search_catalogue` | Search the catalogue by jewellery type, gold colour or diamond style, in French or English. |

Full, self-describing definitions (purpose, usage, behaviour, parameters and output schema) are exposed over MCP by the live server and visible through `tools/list`. Every tool is read-only and returns a typed envelope: `data`, `sources`, `generated_at` and `canonical`.

## Connect

### Claude (web and desktop)

Open Settings, then Connectors, then Add custom connector, and paste the endpoint URL:

```
https://mcp.origineparis.com/mcp
```

No authentication is required. For MCP clients that only speak stdio, the `mcp-remote` bridge works as well:

```json
{
  "mcpServers": {
    "origine-paris": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.origineparis.com/mcp"]
    }
  }
}
```

### ChatGPT

In Settings, open Connectors, enable developer mode, choose Create, and paste the endpoint URL `https://mcp.origineparis.com/mcp` as the MCP server URL (no authentication).

### Cursor

Add the server to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "origine-paris": {
      "url": "https://mcp.origineparis.com/mcp"
    }
  }
}
```

## Listings

- Official MCP registry: `com.origineparis/mcp` ([registry API entry](https://registry.modelcontextprotocol.io/v0.1/servers/com.origineparis%2Fmcp/versions/latest))
- Glama connectors: [com.origineparis/mcp](https://glama.ai/mcp/connectors/com.origineparis/mcp)
- Brand website: [origineparis.com](https://origineparis.com)

## Licence

The contents of this repository (this documentation and the `server.json` manifest) are released under the [MIT Licence](LICENSE), copyright 2026 Origine Paris SAS.

Scope: this licence covers the contents of this repository only. It does not apply to the hosted MCP service at `https://mcp.origineparis.com/mcp`, which remains proprietary to Origine Paris. The data served by the endpoint comes from public sources published by the brand.
