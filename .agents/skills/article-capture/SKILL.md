---
name: article-capture
description: Use when organizing external articles, WeChat posts, blogs, newsletters, forum posts, copied HTML/text, or article screenshots into the blog captures section. Handles public/full-text default, private archive backup, metadata, summaries, and index updates.
---

# Article Capture

Use for external articles, WeChat posts, blogs, newsletters, forum posts, copied HTML/text, screenshots, or article notes.

## Policy

- Default public: if the user does not say it cannot be public, create a public page that may include the full text or cleaned transcript.
- Private override: if the user says it cannot be public, keep full text/screenshots/raw materials only under `_archive`; public pages should contain source, link, summary, key points, and personal notes.
- Always preserve source metadata when available: title, author/publisher, source URL, platform, publication date, capture date.

## Paths

- Public page: `content/captures/articles/<slug>/index.md`
- Private archive: `content/_archive/captures/articles/<slug>/`
- Article index: `content/captures/articles/index.md`
- Captures index: `content/captures/index.md`
- Home, only for important additions: `content/index.md`

Use ASCII slugs and prefer a short durable subject.

## Workflow

1. Save raw input, copied text, screenshots, HTML, or temporary notes to the private archive as backup.
2. Create or update the public page.
3. Public page should include:
   - title, author/publisher, source link
   - content type and topic
   - full text or cleaned transcript when public
   - collection reason
   - key points
   - reusable conclusions
   - personal notes and related internal links
4. Update indexes when the item is worth listing.
5. Validate with `npx --yes @faithleysath/blogx build --profile`.
