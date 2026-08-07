# QA Checklist

Attach evidence to the project QA report. A checkbox without a path, command output, screenshot, or reader note is not evidence.

## Input and scope

- [ ] Source path, size, page count and SHA-256 recorded.
- [ ] Source PDF was not overwritten or re-saved.
- [ ] Title, author, language, script policy, output path and target readers confirmed.
- [ ] Private, licensed, and publication scope recorded.
- [ ] Host OS and architecture recorded; Apple Vision availability checked when required.
- [ ] OCR engines, packages, models, options and offline status recorded.
- [ ] For a Chinese scan, either local PaddleOCR preflight passed with PP-OCRv6 medium det/rec, or an approved user-provided AI Studio `.md`/`.json` route is mapped to the source pages.
- [ ] `work/ocr/engine-preflight.json` records cache, download, run status and failure reasons.
- [ ] Only one PaddleOCR pipeline instance ran for the task; the cache was reused and subagents did not start another OCR runner.
- [ ] Existing project assets and prior QA audited before reprocessing.

## Inventory and images

- [ ] Every PDF page has a stable manifest entry.
- [ ] Single pages and spreads are classified.
- [ ] Every spread has a page-specific gutter/crop record.
- [ ] Covers, spines, title pages, copyright pages, blank pages, illustrations and back matter are classified.
- [ ] Every source page maps to text, an image/special page, or a confirmed blank.
- [ ] Image source page, crop, hash, EPUB location and alternative text are recorded.
- [ ] Image processing is reversible and does not redraw or invent content.

## OCR and proofing

- [ ] All engines received the same page image and crop.
- [ ] Raw JSON was saved for every completed engine/page pair.
- [ ] Candidate text preserves page IDs and reading order.
- [ ] Any AI Studio result is recorded as an external online source with its `.md`/`.json` provenance; if selected, its direct use as the primary candidate is explicit, never mislabeled as local OCR.
- [ ] Difference routing covers missing lines, duplicates, order errors and punctuation/number disagreements.
- [ ] Risk pages were opened against the high-resolution scan.
- [ ] Names, titles, mixed scripts, quotations, ellipses, dashes, numbers, URLs and ISBNs were checked.
- [ ] Cross-page paragraphs, dialogue, gutter edges and illustration neighbors were checked.
- [ ] Final text is frozen separately from raw candidates.
- [ ] Reproducible repairs and one-off decisions are distinguishable.
- [ ] No translation, smoothing, semantic completion or confidence-based auto-selection was used.

## CER evidence

- [ ] Benchmark pages cover ordinary, difficult, mixed-script and illustration-adjacent layouts.
- [ ] Any gold-standard transcript was made independently from the scan.
- [ ] CER uses substitutions plus deletions plus insertions divided by reference characters.
- [ ] Normalization rules are stated and do not hide character or punctuation errors.
- [ ] Invalid, incomplete or mis-paginated blind transcripts are marked invalid.
- [ ] If the conditions are not met, the report says formal CER certification was not obtained.

## EPUB structure

- [ ] Each chapter/semantic section has maintainable XHTML and a visible heading.
- [ ] OPF spine, EPUB 3 navigation and compatibility NCX agree.
- [ ] Cover metadata and cover resource agree.
- [ ] Paper-page anchors are present where required; page-list is not exposed unintentionally.
- [ ] Body text is searchable and resizable.
- [ ] Images have valid references and useful alternative text.
- [ ] Internal links and resource paths resolve.

## Machine and reader checks

- [ ] ZIP integrity passes.
- [ ] mimetype is first and uncompressed.
- [ ] container points to the intended OPF.
- [ ] Custom structure checks pass.
- [ ] EPUBCheck result is recorded verbatim with the tool version.
- [ ] Unique chapter phrases, missing chapters and duplicate sections were searched.
- [ ] Apple Books smoke test completed when in scope.
- [ ] WeChat Reading smoke test completed when in scope.
- [ ] Final EPUB was newly imported rather than read from a stale cache.
- [ ] Different widths, font sizes, line spacing and light/dark backgrounds were checked.

## Handoff

- [ ] EPUB, source tree, tools, manifest and QA report are present.
- [ ] Source and final EPUB hashes are recorded.
- [ ] Raw OCR, candidates, final pages and repair log remain traceable.
- [ ] No private or unlicensed book content is in the public repository.
