# F2 — zero-click XSS via SCRIPTABLE_EXTENSIONS gap

These files are served **inline** by the anonymized-file route but are NOT in
`SCRIPTABLE_EXTENSIONS`, so they do **not** get the sandbox CSP:

- `poc.shtml` -> `text/html`     (full HTML document)
- `poc.svgz`  -> `image/svg+xml` (SVG document, onload + <script>)

## How to trigger (IMPORTANT: use the RAW file URL, not the viewer)
After anonymizing this repo, open the file API URL directly in the browser:

    HOST/api/repo/<REPOID>/file/poc.shtml
    HOST/api/repo/<REPOID>/file/poc.svgz

The viewer (HOST/r/<REPOID>/...) shows *source* and will not execute — you must
hit `/api/repo/.../file/...` so the browser renders the response as a document.

## If it shows as plain text instead of executing
Then the server sent `Content-Type: text/plain` (e.g. a streamer forwarding
GitHub's type). Check with:

    curl -sI 'HOST/api/repo/<REPOID>/file/poc.shtml'

and look at `Content-Type` and whether a `Content-Security-Policy: sandbox...`
header is present. Share those two headers and we'll know exactly what the
deployment does.
