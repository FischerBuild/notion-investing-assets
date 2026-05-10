# Notion Investing — Assets

Stable, publicly-accessible image URLs for the [Investing Notion workspace](https://www.notion.so/354fefc66bb980038c60da4cb32e6594). Used by the cloud routine `trig_01PQ4EV72HgqLQ5PsefhoScT` (YC P26 Daily Monitor) and by `update_page` icon writes from local sessions.

## URL pattern

Each file is referenceable as a Notion page icon via:

```
https://raw.githubusercontent.com/FischerBuild/notion-investing-assets/main/<slug>.png
```

## Slug naming

`<startup_slug>.png` is derived from the canonical Airtable `Startup Name`:
- Lowercase the name.
- Replace any non-alphanumeric character with `_`.
- Collapse runs of `_` into a single `_`.
- Trim leading/trailing `_`.

Examples:
- `Apollo Atomics` → `apollo_atomics.png`
- `Arlo Industries` → `arlo_industries.png`
- `General Instinct` → `general_instinct.png`
- `markup.one` → `markup_one.png`
- `YC (P26) Startup` → `yc_p26_startup.png`
- `9 Mothers` → `9_mothers.png`

## Coverage

Initial commit covers all 167 P26 batch startups that had a Logo attachment in Airtable as of 2026-05-10. The `_mapping.tsv` file records the canonical `name → slug` mapping the Airtable startup table → this repo.

## Adding a new startup

When the daily monitor surfaces a new startup whose slug isn't in this repo:

1. Download the logo from Airtable (Startups table, `Logo` attachment field).
2. Convert to PNG: `sips -s format png input.jpg --out output.png`.
3. Slugify the startup name (rules above) → `<slug>.png`.
4. Commit and push.

A future iteration of the `/sync-pipeline-logos` skill will automate this via `gh` CLI when it detects a routine-created Pipeline row whose corresponding `<slug>.png` is missing from the repo.

## Why GitHub raw and not Notion-hosted?

Notion's API `icon` parameter only accepts emoji or external URLs. Notion-hosted file URLs (uploaded via UI) are scoped to workspace permissions and not publicly resolvable, so they can't be used as icon sources by the cloud routine. GitHub raw URLs are anonymously accessible, served via CDN, and stable indefinitely — perfect for icon URLs.

## Privacy

Logos are public-facing brand assets and are intentionally exposed via this public repo. Do not commit anything sensitive (PII, internal notes, financial data) to this repo.
