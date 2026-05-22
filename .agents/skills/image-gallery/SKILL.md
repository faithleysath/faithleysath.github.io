---
name: image-gallery
description: Use when the user sends or references images, wallpapers, anime images, stickers, memes, screenshots, or asks to collect images into the blog gallery. Requires visual inspection, metadata extraction, public/private handling, archive placement, and gallery index updates for /home/laysath/Projects/faithleysath.github.io.
---

# Image Gallery

Use this skill for images, wallpapers, anime images, stickers, memes, reaction images, screenshots, or visual-reference material intended for the blog gallery.

## Core rule

Inspect every image visually before cataloging it. Do not rely only on filename, URL, or user caption.

Default public policy:

- If the user does not say otherwise, treat the image as public.
- If the user explicitly says it cannot be public, keep the original and metadata under the private archive only.
- If obvious sensitive information is visible, such as accounts, QR codes, IDs, private chats, addresses, locations, unblurred real faces, credentials, or payment details, place it in the private archive first and ask before publishing.

## Paths

- Gallery index: `content/resources/gallery/index.md`
- Public item page: `content/resources/gallery/<yyyy-mm-dd-slug>/index.md`
- Public item assets: `content/resources/gallery/<yyyy-mm-dd-slug>/assets/`
- Private archive: `content/_archive/captures/images/<yyyy-mm-dd-slug>/`
- Metadata template: `content/_archive/templates/image-gallery-item.md`

Use ASCII slugs. Prefer `YYYY-MM-DD-short-subject`, for example `2026-05-22-miku-reaction`.

## Workflow

1. Receive image input.
   - If attached in chat, inspect it directly from the conversation.
   - If a local path is provided, use `view_image` for visual inspection.
   - If a URL is provided, open or download enough to inspect it, then preserve source metadata.

2. Extract file metadata.
   - Use `file`, `sha256sum`, and `stat`.
   - Use `identify` or `magick identify` if available for dimensions, color space, animation, and format details.
   - Use `exiftool` if available; summarize rather than copying noisy raw output.

3. Write metadata.
   - Copy `content/_archive/templates/image-gallery-item.md` into the private archive as `metadata.md`.
   - Fill all fields that can be known.
   - Mark unknown source, author, or license as `待补`.

4. Store files.
   - Keep originals or private-only files in `content/_archive/captures/images/<slug>/`.
   - For public images, copy the publishable image into `content/resources/gallery/<slug>/assets/`.
   - Avoid publishing duplicate huge originals when a resized or web-friendly copy is enough; note any conversion in metadata.

5. Create or update public pages.
   - For each important public image, create `content/resources/gallery/<slug>/index.md`.
   - Append a short row to `content/resources/gallery/index.md`.
   - Update `content/resources/index.md` only when a new category or notable batch is added.
   - Update `content/index.md` only for especially important additions, not every single image.

6. Validate.
   - Run `npx --yes @faithleysath/blogx build --profile`.
   - Check private paths did not publish: `find public -path '*_archive*' -o -path '*_drafts*'`.
   - If `rg -n '_archive|_drafts' public content -g '*.html' -g '*.md'` reports hits, inspect whether they are harmless documentation references or real private path leaks.

## Public item template

```markdown
# <title>

## 元数据

| 字段 | 内容 |
| --- | --- |
| 类型 |  |
| 整理日期 |  |
| 公开状态 | public |
| 来源 |  |
| 作者/发布者 |  |
| 格式/尺寸 |  |
| SHA256 |  |

## 图片

![<alt text>](assets/<file>)

## 视觉描述

## 收藏理由

## 用途标签

## 备注
```
