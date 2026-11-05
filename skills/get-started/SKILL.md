---
name: get-started
description: How to use Listar to find the decision makers of a company, the people who hold a role (including who just took the job), and the professional phone and email of a person, one at a time or for a list. Use when the user asks for B2B contact details, the managers of a company, or new hires in a role.
---

# Getting started with Listar

Listar finds B2B contact details: the decision makers of a company, and the professional email and phone of a named person. Coverage is complete for France (official registers INPI and SIRENE) and partial elsewhere.

## Connection

The plugin adds the Listar MCP server (`https://api.listar.fr/mcp`). The first time, run `/mcp`, pick `listar` and sign in: the browser opens the Listar consent page, where an account can be created. Every search is billed to the Listar credit of the connected organization.

## Typical flow

1. **Company first.** Call `find_company_contacts` with the company. The SIRET of the establishment or its official website domain identify it best; with a name only, add the city. It returns the company and its decision makers (name, job title, LinkedIn profile when known), without their phone or email. Pass `directorsOnly: true` to get only the legal directors in a few seconds.
2. **Then the people you need.** Call `enrich_person` for each contact to reach: first name, last name, company, and the LinkedIn slug when `find_company_contacts` returned one. Ask for one channel only (`enrichmentType`) when the user needs only an email or only a phone.
3. **Read `listarHints`.** When a company is not found, is registered in another city (often the holding of a group or franchise) or has no contacts, the hints say how to refine the search. Tell the user what was found instead of what was asked.

## Companies by description

To build a list of companies from a brief (industry, size, region), call `search_companies`. It answers `pending`: collect it with `get_result` and kind `companies`, then `find_company_contacts` on the companies the user keeps.

## People by role

To find the people who hold a role without knowing their company, call `search_people`: job titles (every form: French and English, both genders, the acronyms), the countries (cities only narrow them), and `startedWithinDays` to keep only who just took the job ("the new heads of sales in France over the last 2 months" is 60). It answers `pending`: collect it with `get_result` and kind `people`. The people come without phone or email, saved as a list: `enrich_list` on its `listId` finds those.

## Lists

For several companies or people, launch every search with `wait: false`, then collect them together with `get_result` (`items`, up to 25 at a time). Searches run in parallel and some take a few minutes.

## Billing

Every result is billed to the Listar credit of the user's organization; a search that finds nothing is free. `get_credit_balance` shows the balance before a large batch. When the credit does not cover a search, the tool answers `insufficient_credit` and delivers nothing.

## Wrong results

When a delivered email bounces or a number belongs to someone else, report it with `report_result_issue` and the id that came with the result. Problems with the connector itself go to `send_feedback`.
