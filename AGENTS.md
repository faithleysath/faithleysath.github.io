# 博客工作区 Agent 指南

这个仓库是 `laysath的博客` 的工作区，使用 BlogX 构建。AGENTS.md 只保留元工作流；具体整理流程由 `.agents/skills/` 承载。

## 基本规则

- 默认工作目录是 `/home/laysath/Projects/faithleysath.github.io`。
- 本文件可能落后于实际仓库状态；如果发现说明和当前文件、命令或构建结果不一致，以实际情况为准，并提醒用户更新本文件。
- BlogX 会发布 `content/` 下的内容；路径中任何以 `_` 开头的目录都视为私有目录，不发布。
- 原始材料、草稿、备份、不能公开或待确认的内容放到 `content/_archive/`。
- 如需 front matter，只使用 BlogX 当前支持的字段：`title`、`layout`、`draft`。
- 除非用户明确要求改视觉效果或主题，否则不要改 `theme/`。
- Markdown 正文相邻无序列表项之间默认空一行；不要把这条应用到 `content/_partials/sidebar.md`。
- `content/_partials/sidebar.md` 应保持紧凑，除非用户明确要求，不要在侧边栏列表项之间加空行。

## 内容入口

- `content/index.md`：首页。
- `content/tech/`：技术折腾、网络、工具、研究、homelab。
- `content/captures/`：外部文章、视频、帖子、播客等收藏整理。
- `content/resources/`：软件、音乐、电子书、网站、工具、素材等资源库。
- `content/resources/gallery/`：图片、壁纸、表情包和视觉素材图库。
- `content/notes/`：课程笔记、概念整理和长期学习材料。
- `content/projects/`：项目列表和想法。
- `content/_archive/`：私有归档，不发布。

## 构建与验证

在仓库根目录运行：

```bash
npx --yes @faithleysath/blogx build --profile
```

期望结果：

- 构建成功
- `warnings: 0`
- 私有目录没有发布到 `public/`

常用检查：

```bash
find public -path '*_archive*' -o -path '*_drafts*'
rg -n '_archive|_drafts' public content -g '*.html' -g '*.md'
```

第二条命令可能命中说明文字，先判断是否真的是私有路径泄漏。

本地预览：

```bash
python3 -m http.server 8000 --bind 0.0.0.0 --directory public
```

常用局域网预览地址：`http://10.251.0.10:8000/`。

## 特殊文件

- `content/tech/network/nftables-nat/index.html` 是独立原始 HTML 页面，编辑时保留 UTF-8 文档头。
