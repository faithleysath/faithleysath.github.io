---
name: resource-catalog
description: Use when cataloging software, ebooks, music, anime, websites, tools, datasets, assets, download sources, or resource lists into the blog resources section.
---

# Resource Catalog

Use for software, ebooks, music, anime, websites, tools, datasets, assets, download sources, and resource lists.

## Paths

- Main resource index: `content/resources/index.md`
- Split pages, when a category grows: `content/resources/<category>/index.md`
- Private backups or sensitive material: `content/_archive/`
- Template: `content/_archive/templates/resource-item.md`

## Rules

- Do not publish private credentials, paid direct-download links, sensitive license files, account data, or private mirrors.
- Prefer adding to `content/resources/index.md` first; split into a category page only when the list becomes large or needs long notes.
- Record name, type, link/acquisition method, use case, evaluation, and status.

## Workflow

1. Determine the primary value of the resource, not just its file format.
2. Add a concise table row or section entry.
3. For larger resources, create a dedicated page and link it from the index.
4. Keep raw notes, receipts, private files, or sensitive access details in `_archive`.
5. Validate with `npx --yes @faithleysath/blogx build --profile`.
