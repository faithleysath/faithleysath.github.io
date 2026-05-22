# 图片私有归档说明

这里保存图库相关的原始图片、不能公开的图片、待确认授权的图片、截图、临时下载和补充材料。`content/_archive/` 不会被 BlogX 发布。

推荐目录：

```text
content/_archive/captures/images/
  <yyyy-mm-dd-slug>/
    original.<ext>
    metadata.md
    notes.md
```

处理规则：

- 每张图必须先做视觉检查，再记录元数据。
- 用户未声明公开状态时，按默认可公开处理。
- 用户明确说不能公开时，只在这里保留原图和元数据，不在公开图库展示原图。
- 如果图里明显有隐私、账号、二维码、身份证件、聊天记录、个人位置、未打码人脸或其他敏感信息，先放这里并等待确认。
- 公开条目引用的图片副本应放到 `content/resources/gallery/<slug>/assets/`。
