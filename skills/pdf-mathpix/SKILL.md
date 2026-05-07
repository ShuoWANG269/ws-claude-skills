---
name: pdf-mathpix
description: Use when the user needs to precisely extract / convert a PDF (especially with math formulas, tables, or scanned content) to Markdown. Triggers on phrases like "精确查看 pdf", "转 pdf 成 md", "mathpix 处理", "把这个 PDF 转一下", "OCR PDF". Calls the local Mathpix-backed `process-pdf` CLI which uses the mpxpy SDK and credentials from ~/.mpx/config.
---

# PDF → Markdown via Mathpix

Wraps the existing local script at `~/Projects/tmp_project/mathpix/` which uses the Mathpix `mpxpy` SDK to convert PDFs (including math, tables) to Markdown.

## When to use

- User wants to read a PDF accurately (formulas / tables / scanned pages) and asks for Markdown extraction.
- User explicitly says "用 mathpix" / "精确转 pdf" / "OCR 这个 PDF".

Do NOT use for: plain text PDFs where `pdftotext` is enough, or when the user only wants a summary (read directly instead).

## Prerequisites (already set up on this machine)

- Wrapper: `~/Projects/tmp_project/mathpix/bin/process-pdf`
- Venv: `~/Projects/tmp_project/mathpix/.venv/` (has `mpxpy` installed)
- Credentials: `~/.mpx/config` (`MATHPIX_APP_ID`, `MATHPIX_APP_KEY`)

If any of these are missing, stop and report — do not try to recreate.

## Usage

```bash
~/Projects/tmp_project/mathpix/bin/process-pdf <input.pdf> [-p PAGE_RANGES] [-o OUTPUT.md] [-t TIMEOUT]
```

- `-p / --page-ranges`: e.g. `1-12`, `2,4-6`, `2 - -2`. Omit to process all pages.
- `-o / --output`: defaults to same dir as input, `.md` extension.
- `-t / --timeout`: seconds, default 200.

## Workflow

1. **Locate the PDF** — if the user gives a name not a full path, `find ~/Documents ~/Downloads -iname "*<name>*.pdf"`.
2. **Confirm scope** — if the PDF is long (>20 pages) and the user did not specify page ranges, ask whether to process all or a range. Mathpix is billed per page.
3. **Run** the wrapper. Stream output so the user sees progress.
4. **Report** the output path and line count.

## Notes

- Each call hits the billed Mathpix API. Don't run "test" calls without user intent.
- Output goes next to the input PDF unless `-o` says otherwise. For files inside `library/` (read-only by vault rules), put the `.md` next to the PDF — that's an additive index file, not a rewrite of the original.
- If `wait_until_complete` times out, retry with a longer `-t` rather than re-uploading.
