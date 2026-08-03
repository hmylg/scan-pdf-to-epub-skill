# Scan PDF to Reflowable EPUB Skills

Bilingual Codex Agent Skills for turning scanned, image-based books into searchable and resizable EPUB projects with traceable page coverage, local OCR candidates, visual proofreading, image preservation, and reader QA.

## Included

- English: .agents/skills/scan-pdf-to-epub/
- 中文：.agents/skills/scan-pdf-to-epub-zh/

The English skill is the default global entry point. The Chinese skill is a parallel Chinese-language entry point with the same workflow and safeguards.

## Use cases

Use these skills for:

- Scanned or image-based PDFs without a reliable text layer.
- Chinese, Japanese, English, and mixed-script books.
- Single-page and two-page scans with variable gutter positions.
- Reflowable EPUBs that preserve images, special pages, and paper-page traceability.
- Incremental repair of an existing scan-to-EPUB project.

The workflow is intentionally evidence-first. OCR output is a candidate, not a finished transcription. The final text must be checked against the high-resolution scan.

## Install from GitHub

In Codex, install the English skill by giving the skill installer this directory URL:

    https://github.com/hmylg/scan-pdf-to-epub-skill/tree/main/.agents/skills/scan-pdf-to-epub

For the Chinese entry point, use:

    https://github.com/hmylg/scan-pdf-to-epub-skill/tree/main/.agents/skills/scan-pdf-to-epub-zh

After installation, restart Codex if the skill does not appear. You can also clone this repository and run Codex from its root so the repository-scoped skills are discoverable.

## Privacy and copyright

This repository contains workflow instructions only. Do not commit private scans, complete OCR, book-specific cover images, copyright pages, or unlicensed EPUB samples. Use synthetic, public-domain, or explicitly redistributable fixtures for tests.

The skills are macOS-first when Apple Vision is requested. On other platforms, the agent must state that the Apple Vision backend is unavailable rather than silently claiming a dual-engine run.

## Status

This is a first public release extracted from a real local workflow. The project favors reproducibility, incremental recovery, explicit uncertainty, and reader-level verification over one-click conversion claims.
