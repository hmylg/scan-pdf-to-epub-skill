# Reader Compatibility Reference

Use this reference when the user names Apple Books, WeChat Reading, or another EPUB reader.

## Principle

EPUBCheck proves package conformance, not reader behavior. Test the final EPUB in each named reader after a fresh import.

## Navigation and cover

- Use EPUB 3 nav.xhtml and a compatibility toc.ncx when older readers are in scope.
- Give each chapter its own XHTML, visible heading, stable heading ID and navigation target.
- Declare the cover image with EPUB 3 cover-image metadata and the compatibility declarations required by the target reader.
- Keep cover, spine/archive page, title page, table of contents, author page and back cover in separate semantic documents.
- Do not let a back-cover image or a front-matter template become the package cover by accident.

## Paper-page anchors

Paper-page anchors can preserve traceability and help readers locate the scan. Keep them in the body where needed, but avoid exposing a page-list unless the user explicitly wants a page-navigation list. Some readers may turn every page-list entry into a noisy table of contents.

## WeChat Reading smoke test

Check that:

- The shelf cover is the front cover, not the back cover.
- Front matter is not repeated as a second table of contents.
- Paper-page anchors do not become hundreds of table-of-contents entries.
- Each chapter has a visible name and opens at the right location.
- Block-level table-of-contents entries wrap and remain separate.

## Apple Books smoke test

Check that:

- The shelf cover and book metadata are correct.
- Chapter names appear in the current-location and navigation UI.
- Front matter, chapter openers and illustrations render without clipping.
- Search finds body text and respects the chosen language/script.
- Resizing text does not hide headings or break image placement.

## General visual pass

Test phone, tablet, and desktop widths when applicable. Change font size, line spacing, theme/background, and orientation. Inspect:

- Chapter titles and long titles.
- Quotation-heavy dialogue and cross-page paragraphs.
- Mixed Chinese, Japanese, English and numeric text.
- Images adjacent to text and page anchors.
- Blank separators and special pages.

Record reader version, platform, import date, test file hash, observations, screenshots if useful, and known limitations. A reader-specific workaround belongs in the project QA report as well as the reusable compatibility notes if it is general.

