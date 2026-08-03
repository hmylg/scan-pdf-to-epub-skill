# Workflow Reference

Use this reference when executing a new conversion or recovering an interrupted project.

## Phase outputs

| Phase | Required output | Stop condition |
| --- | --- | --- |
| Preflight | Source record, hash, page count, platform and user decisions | Metadata or privacy scope is unclear |
| Inventory | Contact sheet, spread/single classification, special-page list | A page cannot be classified safely |
| Manifest | Stable page IDs, crop boxes, states and provenance | Any source page has no planned disposition |
| OCR benchmark | Same-image raw JSON from the selected local engines | Engines cannot run locally or results are not comparable |
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

## Manifest design

Use one stable page ID for the physical source page and separate IDs for left/right crops when a spread is split. Keep the source page relationship explicit.

Recommended state transitions:

1. discovered
2. classified
3. extracted
4. ocr-complete
5. candidate-compared
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

Run the same crop through each local engine. Record engine name, package/model version, options, image hash, runtime date, and raw result path. Make engine upgrades visible in the manifest.

Use a benchmark set that covers layout risk, not just easy prose. The benchmark is for routing and regression; it is not a formal accuracy certificate unless an independent valid gold standard exists.

After candidate comparison:

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

