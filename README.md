# Spain Company Directory MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-spain-company-directory)](https://www.npmjs.com/package/@mambalabsdev/mcp-spain-company-directory) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-spain-company-directory)](https://github.com/mambalabsdev/mcp-spain-company-directory/blob/main/LICENSE)

An MCP server for the Mamba Labs Spain Company Directory actor on Apify. Spanish company data by name, CIF, or sector.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Looks up Spanish companies on infoempresa by name, CIF, or link, or lists companies by search term, sector, status, and legal form, and returns registry data, financials, and officers.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-spain-company-directory": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-spain-company-directory"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `lookup_spain_companies` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Look up Mercadona SA and give me its CIF, address, and officers."
- "List 20 active construction companies in Spain with construcciones in the name."
- "Find the company with CIF A28015865 and its latest revenue."

## Tool

`lookup_spain_companies`. One tool. Example arguments:

```json
{
  "companies": [
    "Telefonica SA"
  ],
  "include_officers": true
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `companies` | array | no | Company names, CIFs (for example A28015865), or infoempresa company links. Each is read from its company page (company-detail-row). A name must match the registered name, legal form aside, unless name_match is closest. |
| `search_terms` | array | no | Words to find in company names, for example construcciones. Each term lists up to about 100 companies (company-row). Combine with the filters below. |
| `status` | string | no | Search filter: any (default), activa, extinguida, or inactiva. |
| `sector` | array | no | Search filter: one or more of infoempresa's 21 sectors by name or number, for example Construcción or Hostelería. With no search term, the filters alone list up to 1,000 companies. |
| `legal_form` | string | no | Search filter: SA, SL, or SLL (or the full name). Empty means any. |
| `max_results_per_search` | number | no | Up to 1,000 for a filter-only search; a search term returns at most about 100. Default 100. |
| `company_details` | boolean | no | true: each search result is read from its company page and charged as a company-detail-row. false (default): search list fields only. |
| `include_officers` | boolean | no | true (default): detail rows carry the officers the company page lists (up to 25). |
| `max_officers_per_company` | number | no | 1 to 25. Default 25. |
| `name_match` | string | no | exact (default): a company name must match the registered name, legal form aside; otherwise a free no_results row lists the closest names. closest: take the first search result. |
| `term_in_name` | boolean | no | true (default): search results whose name lacks a word of the search term are skipped, free. |
| `dedupe` | boolean | no | true (default): a company already returned in this run is skipped, free. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `company-row`, one company from a search list | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `company-detail-row`, one company page read with CIF, officers, and financials | $0.005 | $0.00475 | $0.0045 | $0.00425 |
| `apify-actor-start`, per run start | $0.005 | $0.005 | $0.005 | $0.005 |

Companies not found, repeats, loose matches, summaries, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/spain-company-directory](https://apify.com/mambalabs/spain-company-directory)

## License

MIT. See [LICENSE](LICENSE).
