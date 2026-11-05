# Listar for Claude Code

Official [Listar](https://listar.fr/en) plugin for Claude Code. Find the decision makers of a company and the professional phone and email of a person, from official business registers (INPI, SIRENE) and B2B data sources. Full coverage of France, partial elsewhere.

## Install

```
/plugin marketplace add abrel/claude-plugin
/plugin install listar@listar
```

Then run `/mcp`, pick `listar` and sign in. A Listar account can be created during sign in. Searches are billed to the Listar credit of your organization, and a search that finds nothing is free.

Without the plugin, the MCP server alone:

```
claude mcp add --transport http listar https://api.listar.fr/mcp
```

## What it adds

- The Listar MCP server: `find_company_contacts`, `enrich_person`, `search_companies`, `search_people`, `enrich_list`, `get_result`, lists, credit balance and result reports.
- Skill `get-started`: how to chain the tools.
- Skill `enrich-csv`: enrich a local CSV of people or companies and write the phone, email and decision makers to a new file.

## Examples

- "Find the managers of Games Factory in Quetigny and the email of its president."
- "Enrich leads.csv with the mobile and email of each person."
- "Find the heads of sales who took their job in France over the last two months."

## Data and privacy

- The plugin runs no local code. It declares one remote MCP server, `https://api.listar.fr/mcp`, operated by Listar SAS, and two skills made of instructions only.
- What Claude sends to that server: the search inputs of each tool call (company name, SIREN or SIRET, website domain, city, country, a person's name, company, job title, LinkedIn profile, known email), and for the `enrich-csv` skill the rows of the file the user asks to enrich. The server answers with company details, decision makers, professional phone numbers, emails and LinkedIn profiles.
- Sign in uses OAuth on listar.fr. The access token is an API key of your Listar account, which you can revoke at any time in Settings > API keys.
- Results written by `enrich-csv` stay on your machine, in a new file next to the original.
- Nothing is sent to any other service. Privacy policy: [listar.fr/en/privacy-policy](https://listar.fr/en/privacy-policy).

## Support

[listar.fr/en/contact](https://listar.fr/en/contact). Listar is meant for B2B use, and anyone can object to the processing of their data at any time on listar.fr.
