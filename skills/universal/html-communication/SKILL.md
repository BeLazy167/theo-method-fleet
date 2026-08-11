---
name: html-communication
description: Use when the user asks to communicate through an HTML document, or wants a plan, spec, write-up, findings, summary, report, comparison, or set of UI mocks to read outside the terminal. Also use if they mention HTML with no other context.
metadata:
  harness: [claude, codex]
  platform: [darwin, linux]
  scope: fleet
---

# Communicate in HTML

Terminal output is a bad place to read a long document. Write one self-contained
HTML file, upload it, and hand back the URL.

## Rules

- One file. Self-contained. No external scripts, fonts, or stylesheets. Cap at 512KB.
- Write it like a spec, not a landing page. No hero sections. No marketing copy.
- Dark background, high contrast text. Readable line length. Real headings.
- Keep one file across iterations so the URL stays stable. Edit and re-upload the
  same path instead of creating a new one.
- Never open a browser to check it. Never claim the document is hosted before the
  upload command succeeds.

## UI mocks

When the user asks for design options, render them in one file, labeled `A`,
`B`, `C`. Lay them out for direct comparison, not stacked pages. The user replies
with letters, so the labels must be obvious and unique.

## Upload

```bash
curl -sf -X POST "$POSTPLAN_HOST/upload" \
  -H "Authorization: Bearer $POSTPLAN_TOKEN" \
  -F "file=@plan.html" \
  -F "path=<stable-slug>"
```

Return the URL from the response body. Nothing else.

Requires `POSTPLAN_HOST` and `POSTPLAN_TOKEN`. If either is unset, say so and
give the user the local file path instead. Do not guess a host.
