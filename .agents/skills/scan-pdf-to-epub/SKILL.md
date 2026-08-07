---
name: scan-pdf-to-epub
description: Convert scanned PDFs into traceable, reflowable EPUB projects with page inventory, per-spread splitting, local OCR candidate comparison, visual proofreading, image preservation, navigation construction, EPUBCheck, and reader smoke tests. Use when Codex needs to turn a scanned or image-based PDF into a searchable, resizable EPUB; audit or repair an existing scan-to-EPUB project; or design a privacy-preserving local workflow for Chinese, Japanese, English, or mixed-language books. Do not trigger for ordinary text PDFs, fixed-layout EPUBs, translation-only tasks, or publishing copyrighted scans.
---

# Scan PDF to Reflowable EPUB

## Mission

Turn a scanned or image-based PDF into a searchable, resizable EPUB while keeping every page, image, editorial decision, and validation result traceable to the source. Treat the PDF as an immutable local source. Treat OCR as a candidate generator, not as proof.

## Operating contract

- Keep the source PDF read-only. Never overwrite, re-save, upload, or publish it.
- Work in a separate project directory with src/, work/, tools/, a page-level book-manifest.json, and a human-readable QA report.
- Keep raw OCR JSON, candidate text, final page text, image provenance, and repair notes.
- Preserve covers, title pages, copyright pages, illustrations, QR codes, logos, blank pages, and paper-page anchors.
- Explain that a reflowable EPUB preserves searchable content and images, not the PDF's exact page layout at every width and font size.
- Do not translate, paraphrase, smooth, restore, or invent text that is not supported by the scan.
- Do not treat OCR confidence, model similarity, or language-model fluency as character accuracy.
- Pause before changing the source, uploading content, exposing copyrighted pages, or resolving an ambiguity that requires the owner’s decision.
- State platform limitations honestly. Apple Vision is a local macOS backend; do not claim that a non-macOS run completed the same dual-engine workflow.

## Workflow

### 1. Audit before processing

Read the existing project documentation, scripts, manifest, OCR, final pages, and QA evidence before starting. Reuse completed work and repair incrementally.

Record the source path, file size, SHA-256, page count, page dimensions, rotation, text layer, bookmarks, language, title, author, conversion strategy, output path, and target readers. Report what is already complete and what is missing.

Before full OCR, report:

1. PDF page count and single/spread distribution.
2. Estimated paper-page range and special-page inventory.
3. Existing reusable files and scripts.
4. Planned output tree and evidence.
5. Questions about title, language/script preservation, simplified/traditional conversion, or ambiguous pages.

Do not run full-book OCR until the user confirms the metadata and strategy when they are materially ambiguous.

### 2. Create a page manifest and visual inventory

Create or repair book-manifest.json before generating final text. Give every PDF page a stable ID and a state such as extracted, ocr-complete, proofed, illustration-ready, or confirmed-blank.

For every page, record:

- PDF page number and physical layout (single or spread).
- Image dimensions, rotation, source hash, and crop box.
- Actual gutter position when a spread is split.
- Left/right paper-page numbers, chapter, and special-page type.
- Paths for source image, OCR images, Vision JSON, PaddleOCR JSON, candidate text, final text, and QA evidence.
- Image/illustration location in the EPUB and concise alternative text.

Inspect contact sheets or page previews. Classify covers, spines, title pages, author pages, dedications, copyright pages, chapter openers, illustrations, advertisements, blank separators, and back matter. Every source page must map to final text, a preserved image/special page, or an explicitly confirmed blank page.

Never use a fixed 50/50 split for every spread. Estimate the gutter per page from the dark band, blank band, or text boundary, and retain the crop parameters.

### 3. Acquire independent OCR candidates locally

Use the same page image, resolution, crop, and page ID for every engine. Preserve each engine’s raw JSON with text, confidence, bounding boxes, reading order, version, model, options, and image hash.

On macOS, use Apple Vision with the project’s declared languages and options, commonly zh-Hans, en-US, ja-JP, accurate recognition, and language correction. If the required local framework or permission is unavailable, report the limitation and stop or use an explicitly approved fallback.

For a Chinese scanned or image-based PDF, prefer local PaddleOCR as the baseline and do not treat Apple Vision plus Tesseract as an equivalent substitute. Use PP-OCRv6 medium detection and recognition (`PP-OCRv6_medium_det` + `PP-OCRv6_medium_rec`) on the local route. If the user has already provided official PaddleOCR AI Studio `.md` and `.json` results for this source, record their provenance and page mapping, then use those results directly as the primary OCR candidate; do not force a second local installation or run. PaddleOCR-VL, user-supplied online OCR, or another engine must be named by its actual source.

Before full-book OCR, create `work/ocr/engine-preflight.json` and record for every engine: package/version, model, language, device, options, input count and hash policy, cache/download status, run status, output path, failure reason, and timestamps. If neither local PaddleOCR nor an approved user-provided AI Studio result is available, pause before full OCR and report the concrete blocker. Empty PaddleOCR paths or missing raw JSON are a failed local preflight; they are acceptable only when the approved AI Studio route is recorded with its `.md`/`.json` paths, provenance, and page mapping.

For the default PP-OCRv6 medium baseline, avoid downloading unrelated pipelines unless required: keep document-orientation classification, document unwarping, and text-line orientation disabled unless the benchmark or user requires them. Use one project-managed PaddleOCR environment and one OCR pipeline instance per task; reuse its model cache, do not download or initialize duplicate copies, and keep batch/worker counts bounded so memory use remains predictable. Subagents may review pages or transcribe blind samples, but must not each launch another PaddleOCR runner.

Run a representative benchmark before the full book, using the same images, resolution, crops, and page IDs across engines. Include ordinary prose, dialogue, chapter headings, low-contrast or gutter pages, mixed Chinese/Japanese/English/numeric pages, copyright/metadata pages, and illustration-adjacent pages. Select a primary candidate only after comparing actual layout and error patterns for this book; never hard-code a universal winner.

For a non-sensitive PDF, the user may upload it to the [official PaddleOCR AI Studio](https://aistudio.baidu.com/paddleocr), run its online flagship analysis, download the resulting `.md` and `.json`, and provide their paths to Codex. If those results are supplied before OCR, they may be used directly as the primary candidate and local PaddleOCR installation can be skipped for that task. Record the external provenance and page mapping, do not upload private, confidential, or unauthorized material, and do not call an online result a local PaddleOCR run.

### 4. Route differences to visual proofing

Normalize only what is explicitly allowed for comparison: Unicode NFC, line endings, and leading/trailing whitespace. Use character and line diffs to identify risk pages, missing lines, duplicate lines, order reversals, punctuation disagreements, and numeric/URL differences.

Do not automatically select the longest candidate, the highest-confidence candidate, or the most fluent candidate. Open the high-resolution scan and decide line by line. Freeze decisions in final-pages/ and record reproducible repair rules or a repair log.

Prioritize names, places, titles, Japanese text, quotation marks and nested quotations, ellipses, dashes, numbers, ISBNs, URLs, chapter boundaries, gutter-adjacent lines, cross-page paragraphs, dialogue, and illustration neighbors.

Use a complete scan plus searchable transcription for special pages. Keep decorative typography, logos, barcodes, QR codes, and image text as images when the original page carries meaning.

### 5. Coordinate subagents without contaminating evidence

Use subagents only when helpful and only after the user permits parallel work. Suitable assignments are blind transcription from scan images, independent risk-page review, spread-split inspection, and QA-result review. Do not use subagents to start duplicate OCR environments or duplicate model instances.

Keep blind workers from seeing OCR output, another worker’s transcript, or the frozen final text. Have them return page IDs, transcription, differences, and visual evidence only. Keep the main agent as the sole writer of source files, manifest, final text, EPUB, and delivery report.

Do not describe a blind transcript as a gold standard if page ownership, coverage, or transcription validity is defective.

### 6. Build the EPUB from frozen sources

Build only after final pages and image placement are frozen. Generate, rather than hand-edit, the EPUB from the manifest and source tree.

- Use one maintainable XHTML document per chapter or semantic section.
- Give each chapter a visible <h1>, stable heading ID, semantic structure, and paper-page anchors.
- Keep OPF spine order, nav.xhtml, and compatibility toc.ncx aligned.
- Declare the cover with EPUB 3 cover-image, legacy cover metadata, and the required guide compatibility where needed.
- Keep pagebreak anchors but do not expose a page-list unless the user explicitly needs it.
- Keep the body searchable and resizable; do not lock fonts or bake all text into images.
- Use block-level table-of-contents entries when reader compatibility requires predictable line breaks.

### 7. Validate and report

Run, in proportion to the project:

1. ZIP and mimetype checks.
2. Custom structure checks for container, OPF resources, links, images, headings, spine, and anchors.
3. EPUBCheck 5.3.0 or the project’s declared version.
4. Search checks for chapter phrases, duplicated sections, missing sections, and reversed page ranges.
5. Apple Books and WeChat Reading smoke tests when they are in scope.
6. Visual checks at different widths, font sizes, line spacing, light/dark backgrounds, chapter openers, and image pages.

Report source and output hashes, page coverage, image provenance, engine versions and candidate risks, correction scope, machine-check output, reader results, unresolved risks, and whether formal CER evidence exists. Distinguish structural validity, visual proofreading, and an independent gold-standard CER.

If there is no valid independent gold standard, write that formal CER certification was not obtained. Never turn confidence or candidate similarity into a CER claim.

## Confirmation gates

Stop and ask the user before proceeding when:

- The title page disagrees with the requested title or author.
- Simplified/traditional conversion could change names, Japanese text, titles, or publication data.
- A page may be blank, an illustration, a missing scan, or a split-page ambiguity.
- The scan is obstructed and the glyph cannot be confirmed visually.
- The task would modify, upload, disclose, or publicly distribute source pages or full OCR.
- A cover/back-cover crop would differ from the scan.
- A proposed global replacement is based on semantics rather than scan evidence.

Ordinary implementation steps—creating directories, caching intermediate files, installing declared local dependencies, running checks, and rebuilding generated EPUB files—may proceed without pausing.

## Reference routing

- Read [workflow.md](references/workflow.md) for detailed phase outputs and recovery rules.
- Read [qa-checklist.md](references/qa-checklist.md) before delivery or when auditing an existing QA report.
- Read [reader-compatibility.md](references/reader-compatibility.md) when Apple Books, WeChat Reading, or another reader is in scope.
