---
name: file-upload
description: Use when the user asks to upload a file or share a screenshot, screen recording, log, or build artifact, or when a PR description needs an embedded image or video.
metadata:
  harness: [claude, codex]
  platform: [darwin, linux]
  scope: fleet
---

# Upload a file and get a public URL

Uploads any local file to the user's public file host and returns a URL you can
paste into a PR, an issue, or a chat message.

## Prerequisites

Requires `FILE_HOST_URL` and `FILE_HOST_TOKEN`. If either is unset, tell the user
instead of guessing a host or a token.

## Upload

```bash
curl -sf -X POST "$FILE_HOST_URL" \
  -H "Authorization: Bearer $FILE_HOST_TOKEN" \
  -F "file=@<path>" \
  -F "name=<basename>"
```

Use only the file's base name, such as `login-flow.mp4`. The service slugifies it
and adds a random suffix, so names do not need to be unique.

The response body is the public URL. Return it verbatim. Do not wrap it, shorten
it, or fetch it back to "verify".

## Embedding

- Image: `![login flow](<url>)`
- Video: paste the bare URL on its own line. GitHub renders `.mp4` inline.

A short GIF often reads better than a long video in a PR:

```bash
ffmpeg -i in.mp4 -vf "fps=12,scale=900:-1:flags=lanczos" -loop 0 out.gif
```

Keep GIFs under 10MB. If the recording is long, upload the video and add a GIF of
the key moment.
