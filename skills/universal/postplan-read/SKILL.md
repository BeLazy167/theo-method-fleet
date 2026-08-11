---
name: postplan-read
description: Use when the user provides a postplan.dev URL to read.
metadata:
  harness: [claude, codex]
  platform: [darwin, linux]
  scope: fleet
  requires: "curl"
---

# Postplan Read

Fetch the uploaded HTML with the shell. Do not use web search or a browser.

1. Remove a trailing slash, then append `/raw` unless the URL already ends in
   `/raw`.
2. Run `curl --fail --silent --show-error --location --max-time 30 --output
   /tmp/postplan.html '<raw-url>'`.
3. Read `/tmp/postplan.html` and continue the user's request from its contents.

If `curl` fails, report its actual status or network error. Do not substitute
search results.
