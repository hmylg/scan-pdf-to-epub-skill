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

### Model selection: dated field note

Choose a model according to the current quota, speed, and quality requirements. In the author's personal use as of 2026-08-03, ChatGPT 5.6 Luna Max was sufficient for this workflow and, after a substantial price reduction, was very quota-efficient: a book of about 70,000 Chinese characters used less than 10% of a Plus weekly quota and took roughly 1–2 hours. Sol-Medium/High used quota very quickly—about one full weekly quota for a comparable run. Terra-High produced average results while still consuming a nontrivial amount of quota, making it the least recommended option in this experience. Treat these figures as dated personal observations from that setup, not an accuracy claim, product guarantee, or universal benchmark; re-measure after model, price, or quota changes. A stronger model never replaces visual proofreading against the scan.

### PaddleOCR baseline and optional online analysis

PaddleOCR is a first-class and important engine in this skill, not an optional afterthought. For Chinese scan tasks, the skill installs and uses local PP-OCRv6 medium detection and recognition (`PP-OCRv6_medium_det` + `PP-OCRv6_medium_rec`) by default. If the user has already supplied official PaddleOCR AI Studio `.md` and `.json` results for the source, Codex may use them directly as the primary OCR candidate and skip local installation for that task. The [current official model table](https://github.com/PaddlePaddle/PaddleOCR/blob/main/docs/version3.x/pipeline_usage/OCR.en.md) lists 59.4 MB plus 73.3 MB for the two local weights, about 0.13 GB combined; the reference Mac's current model cache is about 133 MB. Reserve about 1 GB of disk for the Python environment, runtime files, and cache. Runtime RAM is separate and varies with image size, batch size, and process count, so this Skill uses one OCR pipeline instance and forbids duplicate model loads.

For non-sensitive PDFs only, you may upload the file to the [official PaddleOCR AI Studio](https://aistudio.baidu.com/paddleocr), use its official online flagship analysis, download the resulting `.md` and `.json`, and tell Codex their paths. If you provide those results before OCR begins, Codex may use them directly as the primary candidate, so local installation is not required for that task. Record the external provenance and page mapping. Do not upload private, confidential, or unauthorized material, and do not describe an online result as a local PaddleOCR run.

### Parallel work with subagents

For a long book, you can tell Codex: “If needed, you may call subagents.” This can speed up work that can be split into independent checks or blind transcriptions. Keep the skill's privacy and evidence safeguards, and keep the main agent as the sole writer of source files and final decisions.

### Install from GitHub

Use the Codex skill installer with one of these directories:

- [Install the English skill](https://github.com/hmylg/scan-pdf-to-epub-skill/tree/main/.agents/skills/scan-pdf-to-epub)
- [Install the Chinese skill](https://github.com/hmylg/scan-pdf-to-epub-skill/tree/main/.agents/skills/scan-pdf-to-epub-zh)

After installation, start a new Codex turn. Restart Codex only if the skill does not appear. You can also clone this repository and run Codex from its root so the repository-scoped skills are discoverable.

### Privacy and copyright

This repository contains workflow instructions only. Do not commit private scans, complete OCR, book-specific cover images, copyright pages, or unlicensed EPUB samples. Use synthetic, public-domain, or explicitly redistributable fixtures for tests.

The skills are macOS-first when Apple Vision is requested. The author's reference run used a MacBook Air with an M5 chip and Apple's local OCR through Apple Vision. On other platforms, the agent must state that the Apple Vision backend is unavailable rather than silently claiming a dual-engine run.

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

### 模型选择：带日期的经验记录

可以根据当期额度、速度和质量要求选择模型。截至 2026-08-03，作者个人使用发现，ChatGPT 5.6 Luna Max 已经能够满足这套工作流的需求；大幅降价后也非常耐用：制作一本约 7 万中文字的书，消耗不到 10% 的 Plus 周额度，耗时大约 1–2 小时。Sol-Medium/High 会非常快地消耗额度，类似任务差不多会用掉一整个周额度。Terra-High 的效果一般，额度消耗也不低，是这次经验中最不推荐的选项。请把这些数字和判断理解为该环境下带日期的个人观察，不要理解为准确率结论、产品保证或普遍基准；模型、价格或额度变化后应重新测量。无论选择哪个模型，都不能替代对照扫描件的高清视觉校对。

### PaddleOCR 基线与可选在线分析

PaddleOCR 是本 Skill 非常重要的一等引擎，不是可有可无的补充。处理中文扫描 PDF 时，本 Skill 默认在本地安装并使用 PP-OCRv6 medium 检测与识别（`PP-OCRv6_medium_det` + `PP-OCRv6_medium_rec`）。如果用户已经提前提供该来源的官方 PaddleOCR AI Studio `.md` 和 `.json` 结果，Codex 可以直接把它们作为主 OCR 候选使用，本次任务不必安装本地 PaddleOCR。[当前官方模型表](https://github.com/PaddlePaddle/PaddleOCR/blob/main/docs/version3.x/pipeline_usage/OCR.en.md)列出的两个本地模型文件分别约为 59.4 MB 和 73.3 MB，合计约 0.13 GB；参考 Mac 当前的模型缓存约为 133 MB。建议为 Python 环境、运行文件和缓存预留约 1 GB 磁盘空间。运行时内存另行计算，会受图片尺寸、batch 大小和进程数影响，因此本 Skill 只使用一个 OCR pipeline 实例，禁止重复加载模型。

对于不敏感的 PDF，可以上传到[官方 PaddleOCR AI Studio](https://aistudio.baidu.com/paddleocr)，调用官方在线旗舰分析，下载生成的 `.md` 和 `.json` 后把路径告诉 Codex。如果在 OCR 开始前已经提供这些结果，Codex 可以直接把它们作为本次任务的主 OCR 候选使用，因此不必强行安装本地 PaddleOCR。仍需记录外部来源和页面对应关系。不要上传私人、机密或未经授权的内容，也不要把在线结果描述成本地 PaddleOCR 运行结果。

### 使用 subagent 加速

处理较长的书时，可以直接对 Codex 说：“如有需要可以调用 subagent”。对于能够拆分成独立检查或盲录任务的工作，subagent 可以帮助并行处理、缩短耗时。仍需遵守本 Skill 的隐私和证据边界，并让主 agent 作为源文件修改者和最终裁决者。

### 从 GitHub 安装

可以把以下目录 URL 提供给 Codex 的 Skill 安装器：

- [安装英文 Skill](https://github.com/hmylg/scan-pdf-to-epub-skill/tree/main/.agents/skills/scan-pdf-to-epub)
- [安装中文 Skill](https://github.com/hmylg/scan-pdf-to-epub-skill/tree/main/.agents/skills/scan-pdf-to-epub-zh)

安装后开启下一轮 Codex 对话即可使用；只有在 Skill 没有出现在列表中时，才需要重启 Codex。也可以克隆本仓库，并从仓库根目录运行 Codex，让它发现仓库级 Skill。

### 隐私与版权

本仓库只包含工作流说明，不包含项目原始素材。请勿提交私人扫描件、完整 OCR、特定书籍的封面图、版权页或未经授权的 EPUB 样例。测试时请使用合成材料、公版材料或明确允许再分发的样例。

当任务选择 Apple Vision 时，这套 Skill 以 macOS 为主要平台。作者的参考实践使用 MacBook Air M5，并通过 Apple Vision 使用苹果的本地 OCR。在其他平台上，代理必须明确说明 Apple Vision 后端不可用，不应含糊地声称完成了双引擎流程。

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
