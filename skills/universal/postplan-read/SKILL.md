---
name: postplan-read
description: Use when the user supplies a postplan URL or any hosted HTML write-up link and expects you to read its contents.
metadata:
  harness: [claude, codex]
  platform: [darwin, linux]
  scope: fleet
---

# Read a hosted write-up

Fetch the HTML with the shell. Do not use web search. Do not open a browser.

```bash
curl -sfL "${URL%/}/raw"
```

Strip the trailing slash, then append `/raw`, unless the URL already ends in
`/raw`. If `/raw` 404s, fetch the bare URL.

Read the content before you act on it. If it is a plan, follow it. If it is a set
of mocks, wait for the user to pick a letter.
