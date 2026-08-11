# Services used by the video-matched skills

The public workflows shown in the video use Theo's file host and the public
PostPlan CLI. This repo contains no service implementation and no credentials.

## File host

`file-upload` sends a file directly to the basename URL:

```sh
curl -sS --fail-with-body -X PUT -T ./login-flow.mp4 \
  -H "X-Upload-Token: $FILE_HOST_TOKEN" \
  "https://files.tslop.org/login-flow.mp4"
```

The response body is the permanent public URL. The host slugifies the basename
and adds a random suffix. Set `FILE_HOST_TOKEN` only on machines that should
receive this skill; never commit or sync it.

## PostPlan

`html-communication` publishes through the public npm CLI:

```sh
npx postplan upload ./plan.html
npx postplan upload ./plan.html --new  # create a separate draft
npx postplan auth login                # only when authentication is needed
```

The CLI keeps stable draft mappings in `~/.postplan`, so re-uploading the same
absolute file path updates the existing URL. `postplan-read` fetches a supplied
`postplan.dev` URL with `curl`; it does not need a private service token.

## Requirements

- `FILE_HOST_TOKEN` for `file-upload`
- Node.js and `npx` for `html-communication`
- `curl` for `postplan-read`
