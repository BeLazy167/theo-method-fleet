# Services

Two skills call an HTTP endpoint. Neither works until you host one. These are the contracts, so any implementation will do — a Cloudflare Worker over R2, an S3 bucket behind a Lambda, or a small box on the tailnet.

Both are private. Put them behind a bearer token and never commit the token.

## File host

Used by the `file-upload` skill.

**Request**

```
POST $FILE_HOST_URL
Authorization: Bearer $FILE_HOST_TOKEN
Content-Type: multipart/form-data
  file=@<binary>
  name=<basename, e.g. login-flow.mp4>
```

**Response**

`200` with the public URL as the entire body. No JSON wrapper, so the skill can paste the body straight into a PR.

**Behavior**

- Slugify `name` and append a random suffix, so callers never need unique names.
- Serve with the right `Content-Type`. GitHub only renders `.mp4` inline when the type is correct.
- Public read, no listing.

## Write-up host

Used by the `html-communication` and `postplan-read` skills.

**Request**

```
POST $POSTPLAN_HOST/upload
Authorization: Bearer $POSTPLAN_TOKEN
Content-Type: multipart/form-data
  file=@<plan.html>
  path=<stable slug>
```

**Response**

`200` with the public URL as the entire body.

**Behavior**

- The same `path` overwrites in place, so a URL stays stable while a document is iterated on. This is the whole point — the user keeps one tab open.
- `GET <url>` serves the rendered HTML.
- `GET <url>/raw` serves the same bytes as `text/plain`, so an agent can read it with `curl` instead of a browser.
- Cap uploads at 512KB and reject anything that is not HTML.

## Environment

Set these on the machines that should have the skills. `apply-fleet` does not sync secrets.

```sh
export FILE_HOST_URL=...
export FILE_HOST_TOKEN=...
export POSTPLAN_HOST=...
export POSTPLAN_TOKEN=...
```

If a variable is missing, the skills are written to tell you rather than guess a host. Keep it that way.
