# 网页导出（Web Page Export）

一个 **Tampermonkey / ScriptCat 用户脚本**：在任意网页点击 Tampermonkey 图标菜单，把页面**正文**导出为干净的 **Markdown** 或 **HTML** 文件。

基于 **Readability**（正文提取）+ **Turndown**（Markdown 转换）两个成熟库构建，**零手写清洗**、**单一路径**、代码仅约 230 行。

## 特性

- **智能正文提取**：用 Readability 剔除导航、侧栏、页脚、广告，只保留文章主体。
- **双格式导出**：一键导出 `Markdown` 或 `HTML`。
- **元数据自动附带**：标题、作者、摘要、站点、发布时间（来自页面 meta / Readability 提取）。
- **增强的转换**：
  - 标准任务列表 `- [x]` / `- [ ]`
  - 代码块语言标注 ` ```python `
  - 表格竖线转义（GFM 兼容）
  - 懒加载图片回源真实地址
  - `javascript:` 伪协议链接过滤
  - MathJax / KaTeX 数学公式、复杂表格保留为 HTML
- **单一路径**：无模式分叉，代码简洁可维护。

## 安装

1. 安装 [Tampermonkey](https://www.tampermonkey.net/)（或 [ScriptCat](https://scriptcat.org/)）浏览器扩展。
2. 打开 `网页导出.user.js` 原始文件，Tampermonkey 会弹出安装确认页，点击安装。
3. 或在 Tampermonkey 中新建脚本，粘贴本脚本内容保存。

## 使用

1. 打开任意网页（新闻、博客、文档等）。
2. 点击 Tampermonkey 图标，选择菜单：
   - `📄 导出正文为 Markdown`
   - `🌐 导出正文为 HTML`
3. 浏览器自动下载对应的 `.md` / `.html` 文件。

## 依赖

脚本运行时通过 CDN 加载三个库（需能访问 jsdelivr）：

- [Readability.js](https://github.com/mozilla/readability)（正文提取）
- [Turndown](https://github.com/mixmark-io/turndown)（HTML → Markdown）
- [turndown-plugin-gfm](https://github.com/mixmark-io/turndown-plugin-gfm)（表格 / 任务列表等 GFM 扩展）

## 技术要点

```
整页 → Readability 提取正文（keepClasses 保留语言/数学公式 class）
     → Turndown 规则转换（任务列表/代码语言/表格/链接过滤）→ Markdown
     → 或轻量净化后输出 HTML
```

- `keepClasses: true` 保留代码语言、MathJax class，使数学公式与复杂表格在导出后得以保留。
- 任务列表 checkbox 在 Readability 前用 `data-task` 中转保护，提取后恢复。
- 懒加载图片 `data-src`/`data-original` 优先覆盖占位 `src`。

## License

MIT
