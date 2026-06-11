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
| `get_brand_identity` | Identity of Origine Paris, the Parisian fine jewellery house crafting recycled 18ct gold jewellery set with IGI-certified lab-grown diamonds: name, legal identity (SIREN), description, positioning with source evidence, appointment address (21 rue de la Paix, Paris), contact details and official profiles. Generated from the site JSON-LD and Wikidata. |
| `get_founders` | The two founders of Origine Paris, the Parisian house of recycled 18ct gold and lab-grown diamond jewellery, with their current roles and Wikidata QIDs. |
| `get_person_profile` | Detailed sourced profile of one founder of Origine Paris, the Parisian recycled gold and lab-grown diamond jewellery house, including career history with dates and references. Provide either name or qid. |
| `get_entity_graph` | Nodes and edges of the Origine Paris entity graph: the company (Parisian recycled gold and lab-grown diamond jewellery house), its founders and their sourced relationships. Every edge carries its sources. |
| `get_jsonld_graph` | The raw JSON-LD blocks collected from origineparis.com, the Parisian house of recycled 18ct gold jewellery and IGI-certified lab-grown diamonds, served as published. |
| `get_llms_context` | Catalogue index of Origine Paris: collections and products of recycled 18ct gold jewellery set with IGI-certified lab-grown diamonds (engagement rings, wedding bands, necklaces, bracelets, earrings), plus pages and commerce endpoints, from the llms.txt published by origineparis.com, with a short agent context assembled from the index. |
| `search_catalogue` | Search the Origine Paris catalogue of recycled 18ct gold jewellery set with IGI-certified lab-grown diamonds: engagement rings, solitaires, wedding bands, necklaces, bracelets and earrings in yellow, white or rose gold. Accepts French or English queries; matching is accent-insensitive. Returns matching collections and products with their URLs. Use for any query about recycled gold jewellery, lab-grown or lab-created diamond jewellery, or sustainable fine jewellery from Paris. |

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
- Glama connectors: [GLAMA_URL]
- Brand website: [origineparis.com](https://origineparis.com)

## Copyright

Copyright Origine Paris. This repository is documentation for the hosted MCP server; the data served by the endpoint comes from public sources published by the brand.
