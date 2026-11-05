---
name: enrich-csv
description: Enrich a local CSV or spreadsheet of people or companies with Listar, writing the professional phone, email and decision makers into a new file next to the original. Use when the user points to a file of leads, prospects or companies and wants their contact details.
---

# Enrich a local file with Listar

Goal: read a file of people or companies on disk, find their contact details with the Listar MCP tools, and write the results to a new file. The original file is never modified.

## 1. Read the file and map the columns

- Read the file and detect its delimiter and encoding. Show the user the first rows and the mapping you propose.
- **People file**: map first name, last name, company, and when present job title, city, country, LinkedIn URL (keep only the slug after `/in/`) and known email.
- **Company file**: map company name, and when present SIREN, SIRET, website domain, city, country.
- Clean obvious defects before searching: swapped first and last names, a full name in one column (split it), stray spaces, legal forms glued to the name. Skip rows without enough to identify anyone and say how many.

## 2. Confirm the scope and the cost

- Ask which channels are needed: phone, email or both (`enrichmentType`). For companies, ask whether the legal directors are enough (`directorsOnly: true`) or which roles to look for.
- Call `get_credit_balance` and tell the user the base prices given in the tool descriptions. Only what is found is billed. Wait for their go before launching a file of more than 25 rows.

## 3. Launch in batches

- For each row call `enrich_person` (people) or `find_company_contacts` (companies) with `wait: false`, and keep the returned id with the row number.
- Collect with `get_result` and `items` (25 at a time). Searches still running come back `pending`: collect them again a little later.
- For a company file where the user wants the phone or email of the contacts, call `enrich_person` on the contacts kept, passing `linkedinSlug` when returned.

## 4. Write the output

- Write `<original name>_listar.csv` next to the input: every original column, then `listar_phone`, `listar_phone_usage`, `listar_email`, `listar_linkedin`, `listar_status` (found, not found, ambiguous) and `listar_hint` (the `listarHints` text when any). For companies, one row per contact with the company columns repeated.
- Never invent or guess a value: a field Listar did not return stays empty.
- End with a short summary: rows processed, phones found, emails found, rows not found and the main reasons from `listarHints`, credit used.

## Wrong results

When the user says a delivered email bounced or a number belongs to someone else, call `report_result_issue` with the id of that result.
