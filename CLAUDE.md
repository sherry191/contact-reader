# vCard Reader

## Project Overview

A single-file, client-side web app that parses `.vcf` (vCard) files and displays contact info as plain text. Built for non-technical users — no install, no server, no dependencies. Users drag & drop or browse to upload vCard files and can copy the results.

## Live URL

https://sherry191.github.io/contact-reader/

## Stack

- Pure HTML + vanilla JavaScript — no frameworks, no build step, no npm
- Hosted on GitHub Pages (`sherry191/contact-reader` repo, `main` branch, root `/`)

## Key File

| File | Description |
|------|-------------|
| `index.html` | The entire app — HTML, CSS, and JS in one file |

## Architecture

Everything runs in the browser. No server-side logic.

**Parsing flow:**
1. User uploads `.vcf` file(s)
2. `FileReader` reads file as text
3. `parseVCF()` splits on `BEGIN:VCARD` blocks, extracts fields line by line
4. `renderCard()` builds a card element and a plain-text string per contact
5. User clicks Copy to copy plain text to clipboard

**vCard fields parsed:** `FN`, `N`, `ORG`, `TITLE`, `EMAIL`, `TEL`, `ADR`, `URL`, `NOTE`, `BDAY`

## Limits & Validation

| Rule | Value | Where enforced |
|------|-------|----------------|
| Max files per upload | 20 | `handleFiles()` — `MAX_FILES` constant |
| Max file size | 1MB | `handleFiles()` — `MAX_FILE_SIZE` constant |
| File type | `.vcf` or `text/vcard` only | `handleFiles()` filter |

## Security

- **Content Security Policy** — `<meta http-equiv="Content-Security-Policy">` in `<head>`. Blocks external scripts and unauthorized inline execution.
- **HTML escaping** — all vCard values passed through `escHtml()` before being written to the DOM via `innerHTML`
- **URL sanitization** — `sanitizeUrl()` strips `javascript:` and `data:` protocol URLs before render
- **No server** — no attack surface for injection, auth bypass, or data exfiltration

## Deploying Changes

```bash
# edit index.html, then:
git add index.html
git commit -m "your message"
git push
# GitHub Pages auto-deploys from main — live in ~1 min
```

## Common Changes

**Adjust file size or count limits** — change `MAX_FILES` or `MAX_FILE_SIZE` at the top of the `<script>` block.

**Add a new vCard field** — add a `case 'FIELDNAME':` to the `switch` in `parseVCF()`, store it on the `contact` object, then add a render line in `renderCard()`.

**Change output format** — modify the `textLines.push(...)` calls in `renderCard()` to adjust how plain text is formatted.
