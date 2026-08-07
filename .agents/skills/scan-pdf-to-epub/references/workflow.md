# Workflow Reference

Use this reference when executing a new conversion or recovering an interrupted project.

## Contents

- [Phase outputs](#phase-outputs)
- [Preflight record](#preflight-record)
- [PaddleOCR route preflight for Chinese scans](#paddleocr-route-preflight-for-chinese-scans)
- [Manifest design](#manifest-design)
- [Spread splitting](#spread-splitting)
- [OCR and candidate routing](#ocr-and-candidate-routing)
- [Proofing record](#proofing-record)
- [Build and recovery](#build-and-recovery)
- [Privacy and publication](#privacy-and-publication)

## Phase outputs

| Phase | Required output | Stop condition |
| --- | --- | --- |
| Preflight | Source record, hash, page count, platform and user decisions | Metadata or privacy scope is unclear |
| Inventory | Contact sheet, spread/single classification, special-page list | A page cannot be classified safely |
| Manifest | Stable page IDs, crop boxes, states and provenance | Any source page has no planned disposition |
| OCR acquisition | Route A: local PP-OCRv6 medium plus Apple Vision on macOS; Route B: mapped AI Studio exports plus Apple Vision on macOS; optional alternate backend offered on non-macOS | Neither route’s PaddleOCR candidate is available |
| Full OCR | Cached candidates and resumable logs | A batch would overwrite prior raw output |
| Proofing | Frozen final-pages, visual decisions and repair log | Text is chosen only by confidence or fluency |
| EPUB build | Generated source tree, EPUB package and build log | Final pages or image placement are still changing |
| QA | Machine checks, reader smoke tests and unresolved-risk list | A passing ZIP is being mistaken for reader compatibility |

## Preflight record

Keep a machine-readable source record containing:

- Absolute source path, file size, page count and SHA-256.
- File format, embedded text layer, bookmarks, rotation and page dimensions.
- Title, creator, language, script-preservation rules and simplified/traditional policy.
- Reflowable versus fixed-layout target and target reader list.
- Output project path and whether the work is private, licensed, or intended for publication.
- Host OS, architecture, local OCR versions, model versions and network/offline status.

Never place a private source path or a cloud credential in a public report.

## PaddleOCR route preflight for Chinese scans

Choose exactly one route before full processing:

- **Route A — local PP-OCRv6 medium:** Require PaddleOCR 3.7 or newer and use `PP-OCRv6_medium_det` plus `PP-OCRv6_medium_rec`. Compare it with Apple Vision on macOS. On non-macOS, complete the PP-OCRv6 medium candidate, disclose that Apple Vision is unavailable, then ask whether to add another available OCR backend. Continue with visual proofing if the user declines.
- **Route B — user-supplied AI Studio:** When the user supplied official PaddleOCR AI Studio `.md` and `.json` exports before processing, record their provenance, hashes, export metadata when available, and complete page mapping. Use them directly as the PaddleOCR candidate without installing local PaddleOCR. Compare them with Apple Vision on macOS. On non-macOS, disclose that Apple Vision is unavailable, then ask whether to add another available OCR backend; continue with visual proofing if the user declines.

Before full processing, create `work/ocr/engine-preflight.json` and record the selected route. For every available engine or external export, record source, package/version when applicable, model when known, language, device, options, input count and hash policy, cache/download status, run status, output path, failure reason, and timestamps. A Chinese task is not ready when neither route is available. Missing local PaddleOCR raw JSON is acceptable only on Route B with recorded `.md`/`.json` paths, provenance, hashes, and page mapping.

On Route A, use one project-managed PaddleOCR environment and one pipeline instance per task. Reuse the model cache, keep batch/worker counts bounded, and do not let subagents or parallel branches initialize duplicate model copies. Unless a benchmark or user requires them, disable document orientation classification, document unwarping, and text-line orientation so the baseline stays on PP-OCRv6 medium det/rec.

For non-sensitive PDFs, the user may run the [official PaddleOCR AI Studio](https://aistudio.baidu.com/paddleocr), download its `.md` and `.json`, and provide the paths to Codex for Route B. Never upload private, confidential, or unauthorized material, and never label this external result as a local PaddleOCR run.

## Manifest design

Use one stable page ID for the physical source page and separate IDs for left/right crops when a spread is split. Keep the source page relationship explicit.

Recommended state transitions:

1. discovered
2. classified
3. extracted
4. ocr-complete
5. candidate-compared, or primary-candidate-recorded when a non-macOS user declines another OCR backend
6. proofed
7. illustration-ready or confirmed-blank
8. epub-placed
9. qa-checked

Do not skip from discovered to proofed. If a state is not applicable, record the reason rather than deleting the page.

## Spread splitting

For each spread:

1. Inspect the full-resolution page.
2. Locate the actual gutter using the dark band, whitespace, page edges, or text boundary.
3. Record the crop box and any rotation.
4. Save the untouched physical page and the derived OCR crops.
5. Inspect the crop edges for missing characters, shadow intrusion, and reading-order changes.

Re-run OCR only for pages whose crop or image hash changed. Keep prior candidates for comparison.

## OCR and candidate routing

On Route A, run PP-OCRv6 medium after preflight and run Apple Vision on the same crop on macOS. On non-macOS, ask about another available OCR backend only after the PP-OCRv6 medium candidate exists. Record engine name, package/model version, options, image hash, runtime date, and raw result path. Make engine upgrades visible in the manifest.

On Route B, preserve the supplied `.md` and `.json` unchanged, map them to source pages, and record any unknown online preprocessing. On macOS, run Apple Vision on the mapped source pages for comparison. On non-macOS, ask whether to add another available OCR backend. Do not manufacture same-image comparability or run local PaddleOCR merely to duplicate AI Studio.

Use a risk set that covers layout risk, not just easy prose. With two candidates it supports candidate routing and regression; with one accepted candidate it drives high-resolution visual proofing. It is not a formal accuracy certificate unless an independent valid gold standard exists.

After candidate comparison, or after Route B risk sampling:

- Queue pages with missing or extra lines.
- Queue pages with reversed columns or spread order.
- Queue pages with disagreement in names, punctuation, numbers, URLs or mixed scripts.
- Search the whole candidate set for repeated error patterns.
- Open the scan before committing any correction.

## Proofing record

For each material decision, keep the page ID, scan region or visual note, old candidate, final text, reviewer, date, and reason. A deterministic replacement is acceptable only when the scan evidence and affected-page list are explicit.

Do not globally normalize quotation marks, dashes, ellipses, full-width forms, or variant characters unless the user has requested that policy and the change is demonstrably safe. Cross-page dialogue is a frequent source of false positives.

## Build and recovery

Build from frozen page text and the manifest. Generated XHTML should be disposable; source text, image metadata, configuration, and repair logs should be recoverable.

For interrupted work:

1. Read the manifest and build log.
2. Compare image hashes and engine/version metadata.
3. Keep valid raw output.
4. Re-run only missing or invalid states.
5. Re-run candidate diff and QA after any source-page change.

A generated EPUB is not the source of truth. The source tree must be sufficient to rebuild it.

## Privacy and publication

Keep private scans, complete OCR, cover images, copyright pages, and book-specific transcripts outside a public repository unless the user has rights and explicitly approves publication. Public fixtures should be synthetic, public-domain, or licensed for redistribution.
