# Edge Impulse classifier — reusable web template

Works with any Edge Impulse **image classification** model. Nothing in
`index.html` is tied to a particular model: the class names, the input size and
the confidence threshold are all read from the model itself at load time.

## Using it for a new model

1. In Edge Impulse: **Deployment → WebAssembly → Build**, then unzip.
2. Copy these two files out of the zip's `browser/` folder into this folder,
   **next to `index.html`**:
   - `edge-impulse-standalone.js`
   - `edge-impulse-standalone.wasm`
   - `run-impulse.js`
3. Open `index.html` and fill in the three marked spots:
   - `FILL IN #1` — the browser tab title
   - `FILL IN #2` — the heading and subtitle
   - `FILL IN #3` — the `CONFIG` block (file-size limit, threshold, emoji)
4. Run `python3 server.py` and open <http://localhost:8082>.

That's it. Class names, colours and bars appear automatically.

## Why the server

The `.wasm` file must be served over http with the `application/wasm` content
type. Double-clicking `index.html` opens it as `file://` and the model will not
load. Any real host (Netlify, Vercel, GitHub Pages, nginx) already does this
correctly — `server.py` is only for working locally.

## If something breaks

| What you see | What it means |
|---|---|
| `404 edge-impulse-standalone.wasm` | the `.wasm` isn't next to the `.js` |
| `Incorrect response MIME type` | not served as `application/wasm` (you opened it as a file) |
| `EdgeImpulseClassifier is not defined` | script tags in the wrong order |
| `Classification failed (err code: -6)` | feature count is wrong — this template only handles **RGB image** models |

## Non-image models

For audio or sensor models the page layout still works, but the upload/canvas
part does not. Replace the block marked `// ── 3.` and the feature loop in
`// ── 4.` with your own sample capture. Everything must add up to
`props.input_features_count` values.
