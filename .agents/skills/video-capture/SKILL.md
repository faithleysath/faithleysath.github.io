---
name: video-capture
description: Use when organizing Bilibili videos, online videos, podcasts, transcripts, screenshots, danmaku excerpts, or timestamped notes into the blog captures section.
---

# Video Capture

Use for Bilibili videos, online videos, podcasts, transcripts, screenshots, danmaku excerpts, or timestamped notes.

## Policy

- Default public: if the user does not say it cannot be public, public pages may include transcripts, excerpts, screenshots, timestamps, and notes.
- Private override: if the user says it cannot be public, keep transcript/raw media/screenshots only under `_archive`; public pages should contain link, summary, timestamps, key points, and personal notes.

## Paths

- Public page: `content/captures/videos/<slug>/index.md`
- Private archive: `content/_archive/captures/videos/<slug>/`
- Captures index: `content/captures/index.md`
- Home, only for important additions: `content/index.md`

Create `content/captures/videos/index.md` if video entries become common.

## Workflow

1. Record URL, title, uploader/publisher, platform, publish date, and capture date.
2. Archive transcript, screenshots, danmaku excerpts, temporary notes, and raw metadata.
3. Create a public page for standalone videos; otherwise add a short entry to an index page.
4. Public page should include:
   - source metadata and link
   - transcript or excerpts when public
   - key timestamps
   - core ideas
   - reusable conclusions
   - personal notes and related internal links
5. Validate with `npx --yes @faithleysath/blogx build --profile`.
