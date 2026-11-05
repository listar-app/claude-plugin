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

## Support

[listar.fr/en/contact](https://listar.fr/en/contact). Listar is meant for B2B use, and anyone can object to the processing of their data at any time on listar.fr.
