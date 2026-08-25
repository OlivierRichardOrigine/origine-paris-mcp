# Origine Paris MCP server

Official Model Context Protocol (MCP) server of [Origine Paris](https://origineparis.com), the Parisian fine jewellery house crafting recycled 18ct gold jewellery set with IGI-certified lab-grown diamonds. Collection pieces (engagement rings, wedding bands, necklaces, bracelets, earrings) are ordered online at origineparis.com; bespoke creations are designed with the house, by appointment at 21 rue de la Paix, Paris.

This repository documents the hosted server. The server is public and strictly read-only: it serves brand identity and catalogue data generated from public sources, namely the JSON-LD published on origineparis.com, the site llms.txt and Wikidata ([Q139905888](https://www.wikidata.org/wiki/Q139905888)). Anything absent from those sources is reported as unknown, never invented.

## Endpoint

| | |
| --- | --- |
| URL | `https://mcp.origineparis.com/mcp` |
| Transport | Streamable HTTP |
| Authentication | None required |
| Access | Public, read-only |

Health check: `https://mcp.origineparis.com/health`. Discovery manifest: `https://mcp.origineparis.com/.well-known/mcp/server.json` (also served at `/.well-known/mcp.json`), mirrored in this repository as [server.json](server.json).

## Tools

Three read-only tools, plus six deprecated aliases kept until 1.0.0.

| Tool | Description |
| --- | --- |
| `get_maison` | The house and its founders, one section at a time, in the requested language (`lang`: `fr` or `en`). Sections: `overview` (identity, legal identity (SIREN), description, positioning, the by-appointment address, contacts, official profiles, how orders are placed), `founders`, `person` (one founder's full profile; pass `person`), `entity_graph`, `jsonld` (the raw JSON-LD as published, per language), `agent_context`, `llms_txt` (the site llms.txt, French only). Every field that can be missing carries its value, status, source URL and date; content the site does not publish in the requested language is reported as not published, never translated. |
| `get_product_detail` | Everything the site publishes about one piece (`lang` required; `handle` or the product URL, or `product_id` as a Shopify gid; optional `size`), each field with its status, source and date. The description is the published text, never parsed. The price is the one published on the site at the date of reading, and it is the price payable at the online checkout while the site publishes that the catalogue is bought online. Whether a piece can be bought online is read from the site itself, never guessed. Bespoke pieces and sizes outside the published range come with a consultation block. |
| `search_catalogue` | Structured search over the published catalogue (`lang` required; free-text `query` plus the filters `jewellery_type`, `collection`, `gold_colour`, `min_price`, `max_price`; `limit` and `cursor`). The jewellery type, given or detected in the query, is a tier: a piece of another type never precedes a piece of the requested type. Every result carries its score, the maximum possible and the reasons behind it. Collections come back as facets rather than results, and pagination is stateless. Bespoke intents and queries with no match come with a consultation block. |

Deprecated aliases, answering exactly as before plus a deprecation notice, removed in 1.0.0: `get_brand_identity` (overview), `get_founders` (founders), `get_person_profile` (person), `get_entity_graph` (entity_graph), `get_jsonld_graph` (jsonld), `get_llms_context` (llms_txt).

Full, self-describing definitions (purpose, usage, behaviour, parameters and output schema) are exposed over MCP by the live server and visible through `tools/list`. `get_maison` returns a typed envelope: `data`, `provenance` (sources with fetch dates, index date, last synchronisation success), `freshness`, `canonical` and `notices`; the other tools return `data`, `sources`, `generated_at` and `canonical`.

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
