# Notion Investing — Assets

Stable, publicly-accessible image URLs for the [Investing Notion workspace](https://www.notion.so/354fefc66bb980038c60da4cb32e6594). Used by cloud routines and `update_page` icon writes from local sessions.

## Structure

```
notion-investing-assets/
├── p26/            # YC P26 batch — 167 startup logos (initial, sourced from Airtable)
├── w26/            # YC W26 batch — older batch, populated as needed
├── portfolio/      # Angel investments outside YC (logos scraped from company websites)
├── funds/          # Fund logos (Seedcamp, etc.) — scraped from fund websites
└── advisory/       # People / org logos for Advisory tasks
```

Empty subdirectories contain `.gitkeep` so the structure is visible until logos arrive.

## URL pattern

```
https://raw.githubusercontent.com/FischerBuild/notion-investing-assets/main/<category>/<slug>.png
```

Examples:
- `https://raw.githubusercontent.com/FischerBuild/notion-investing-assets/main/p26/apollo_atomics.png`
- `https://raw.githubusercontent.com/FischerBuild/notion-investing-assets/main/portfolio/<slug>.png`
- `https://raw.githubusercontent.com/FischerBuild/notion-investing-assets/main/funds/seedcamp.png`

## Slug rules

`<slug>` is derived from the canonical entity name:
- Lowercase
- Replace any non-alphanumeric character with `_`
- Collapse runs of `_` into a single `_`
- Trim leading/trailing `_`

Examples: `Apollo Atomics → apollo_atomics`, `markup.one → markup_one`, `9 Mothers → 9_mothers`, `YC (P26) Startup → yc_p26_startup`.

## Sources by category

| Category | Source for new logos |
|----------|----------------------|
| **p26**, **w26**, future YC batches | Airtable Startups table → `Logo` attachment field. Daily monitor routine references the URL via slug. |
| **portfolio** | Scraped from the company's website on add. Fallback chain: `<link rel="apple-touch-icon">` → `<link rel="icon" sizes="...">` → `<meta property="og:image">` → `/favicon.ico`. |
| **funds**, **advisory** | Same scraping flow as portfolio, against the entity's homepage. |

## Adding a new logo

When the daily monitor (or another local skill) detects a Notion row whose icon URL would 404 against this repo, the corresponding logo is fetched from the source above, slugified, and committed. See `.claude/skills/sync-pipeline-logos/SKILL.md` and (forthcoming) `.claude/skills/sync-portfolio-logos/SKILL.md` for the automation details.

## Why GitHub raw and not Notion-hosted?

Notion's API `icon` parameter only accepts emoji or external URLs. Notion-hosted file URLs (uploaded via UI) are workspace-permissioned and not publicly resolvable, so the cloud routine can't use them. GitHub raw URLs are anonymously accessible, served via CDN, and stable indefinitely. Notion fetches and caches icon images on first display so the URL resolves quickly thereafter.

## Privacy

Logos are public-facing brand assets and are intentionally exposed via this public repo. Do not commit anything sensitive (PII, internal notes, financial data) to this repo.
