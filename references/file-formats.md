# DOCX and PDF workflow

Use this workflow only when the manuscript or author sample is a `.docx` or `.pdf`. The academic
editing rules in `SKILL.md` remain authoritative; this reference adds file handling and verification.

## Choose the deliverable

- Preserve the source file. Write to a new, clearly named output path.
- For DOCX input, default to an edited DOCX. Preserve the document's styles, sections, headers,
  footers, lists, tables, figures, captions, footnotes/endnotes, comments, tracked changes, fields,
  hyperlinks, bookmarks, and equation representation unless the user asks to change them.
- For PDF input, treat the PDF as a reading source, not an editable document model. If the user does
  not specify an output format, return the cleaned text and change report. If they request a file,
  prefer DOCX for an editable deliverable or create a new PDF when they explicitly need PDF. Explain
  when exact source-layout reproduction is not reliable, especially for multi-column, form, or scanned
  PDFs.
- Do not silently convert a signed, encrypted, or fillable PDF. Report the constraint and ask before
  changing signatures, permissions, or form behavior.

## DOCX input

Use Codex's `documents` skill for reading, editing, rendering, and verification.

1. Inspect the whole document, including tables, headers/footers, footnotes/endnotes, comments, tracked
   changes, text boxes, equations, fields, and embedded figures. Determine which prose is in scope.
2. Edit only in-scope prose. Prefer local run- and paragraph-level changes so formatting and anchors
   survive. Do not rebuild the document from extracted plain text.
3. Preserve all numbers, citation markers and keys, cross-references, equations, symbols, and field
   codes. Do not edit bibliography entries unless the user explicitly includes them.
4. Compare the source and output structurally. Confirm that section, table, figure/image, note,
   comment, relationship, equation, and field counts are unchanged unless an intentional edit explains
   the difference. Compare every number and citation token in the edited scope.
5. Render the edited DOCX to page PNGs with the `documents` skill's bundled renderer. Inspect every
   page at full size, correct any clipping, overlap, missing glyphs, table drift, or page-break damage,
   and render again after each fix.

If tracked changes are requested, use real Word tracked revisions. Otherwise produce a clean edited
copy and summarize changes separately.

## PDF input

Use Codex's `pdf` skill for extraction, rendering, inspection, and final verification.

1. Inspect document metadata and security/signature/form state. Render every relevant page to images
   before relying on extracted text so that columns, captions, footnotes, equations, and reading order
   are understood.
2. Extract text with layout information. For multi-column pages, tables, sidebars, or positioned text,
   reconcile extraction against the page images rather than trusting raw extraction order.
3. Detect image-only or low-text pages. OCR them when an OCR path is available, retain page boundaries,
   and flag uncertain characters instead of guessing. Pay particular attention to minus signs,
   superscripts/subscripts, Greek letters, equations, reference numbers, and decimal punctuation.
4. Apply the academic editing pass only after the source text is reliable. Keep page/section provenance
   during the audit so every proposed edit can be located in the source.
5. For a requested DOCX, create an editable document that preserves logical structure and clearly notes
   any unavoidable layout differences. For a requested PDF, create a new PDF from the cleaned content;
   do not claim pixel-identical layout unless it was actually preserved and verified.
6. Render the final DOCX or PDF and inspect every page. Re-extract final text and compare every number,
   citation token, equation, and symbol with the source. Resolve any discrepancy before delivery.

## Change report additions

For file inputs, also state:

- source and delivered format;
- whether OCR was required and any unresolved low-confidence text;
- whether layout or interactive features changed;
- that all pages were rendered and inspected; and
- whether the number/citation/equation preservation checks passed.
