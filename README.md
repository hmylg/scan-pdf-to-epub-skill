# Scan PDF to Reflowable EPUB Skills

[English](#english) · [中文](#中文)

This repository provides two bilingual Codex Agent Skills for turning scanned, image-based books into searchable and resizable EPUB projects with traceable page coverage, local OCR candidates, visual proofreading, image preservation, and reader QA.

## English

### Included

- [English skill](.agents/skills/scan-pdf-to-epub/)
- [中文 skill](.agents/skills/scan-pdf-to-epub-zh/)

The English skill is the default global entry point. The Chinese skill is a parallel Chinese-language entry point with the same workflow and safeguards.

### Use cases

Use these skills for:

- Scanned or image-based PDFs without a reliable text layer.
- Chinese, Japanese, English, and mixed-script books.
- Single-page and two-page scans with variable gutter positions.
- Reflowable EPUBs that preserve images, special pages, and paper-page traceability.
- Incremental repair of an existing scan-to-EPUB project.

The workflow is intentionally evidence-first. OCR output is a candidate, not a finished transcription. The final text must be checked against the high-resolution scan.

### Install from GitHub

Use the Codex skill installer with one of these directories:

- [Install the English skill](https://github.com/hmylg/scan-pdf-to-epub-skill/tree/main/.agents/skills/scan-pdf-to-epub)
- [Install the Chinese skill](https://github.com/hmylg/scan-pdf-to-epub-skill/tree/main/.agents/skills/scan-pdf-to-epub-zh)

After installation, start a new Codex turn. Restart Codex only if the skill does not appear. You can also clone this repository and run Codex from its root so the repository-scoped skills are discoverable.

### Privacy and copyright

This repository contains workflow instructions only. Do not commit private scans, complete OCR, book-specific cover images, copyright pages, or unlicensed EPUB samples. Use synthetic, public-domain, or explicitly redistributable fixtures for tests.

The skills are macOS-first when Apple Vision is requested. On other platforms, the agent must state that the Apple Vision backend is unavailable rather than silently claiming a dual-engine run.

### Repository layout

```text
.agents/skills/
├── scan-pdf-to-epub/       # English entry point
└── scan-pdf-to-epub-zh/    # 中文入口
```

### License

The original workflow instructions are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). See [LICENSE.txt](LICENSE.txt).

### Status

This is a first public release extracted from a real local workflow. The project favors reproducibility, incremental recovery, explicit uncertainty, and reader-level verification over one-click conversion claims.

## 中文

本仓库提供两个双语 Codex Agent Skill，用于把扫描版、图片型书籍制作成可搜索、可调字号的 EPUB，并保留逐页对应关系、本地 OCR 候选、高清扫描核校、插图和特殊页面，以及阅读器验收证据。

### 包含内容

- [英文 Skill](.agents/skills/scan-pdf-to-epub/)
- [中文 Skill](.agents/skills/scan-pdf-to-epub-zh/)

英文 Skill 是默认的全球用户入口；中文 Skill 使用同一套工作流和安全边界，但提供中文说明与中文默认提示词。

### 适用场景

适用于：

- 没有可靠文字层的扫描版或图片型 PDF。
- 中文、日文、英文及混合文字书籍。
- 裁切位置不固定的单页扫描和双页扫描。
- 需要保留插图、特殊页面和纸书页码对应关系的可重排 EPUB。
- 对已有 PDF→EPUB 项目进行增量修复和质量审计。

本工作流以证据为先：OCR 只是候选结果，不是最终录入文本；最终文字必须对照高清扫描件逐页核校。

### 从 GitHub 安装

可以把以下目录 URL 提供给 Codex 的 Skill 安装器：

- [安装英文 Skill](https://github.com/hmylg/scan-pdf-to-epub-skill/tree/main/.agents/skills/scan-pdf-to-epub)
- [安装中文 Skill](https://github.com/hmylg/scan-pdf-to-epub-skill/tree/main/.agents/skills/scan-pdf-to-epub-zh)

安装后开启下一轮 Codex 对话即可使用；只有在 Skill 没有出现在列表中时，才需要重启 Codex。也可以克隆本仓库，并从仓库根目录运行 Codex，让它发现仓库级 Skill。

### 隐私与版权

本仓库只包含工作流说明，不包含项目原始素材。请勿提交私人扫描件、完整 OCR、特定书籍的封面图、版权页或未经授权的 EPUB 样例。测试时请使用合成材料、公版材料或明确允许再分发的样例。

当任务选择 Apple Vision 时，这套 Skill 以 macOS 为主要平台。在其他平台上，代理必须明确说明 Apple Vision 后端不可用，不应含糊地声称完成了双引擎流程。

### 仓库结构

```text
.agents/skills/
├── scan-pdf-to-epub/       # English 入口
└── scan-pdf-to-epub-zh/    # 中文入口
```

### 许可证

原创工作流说明采用 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 许可，详见 [LICENSE.txt](LICENSE.txt)。

### 项目状态

这是从真实本地工作流整理出的首个公开版本。本项目重视可复现性、增量恢复、明确表达不确定性，以及阅读器级别的验证，不把它宣传成“一键转换”工具。
