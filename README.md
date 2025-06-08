# OBS Browser Source: Scene Trigger

A simple static HTML page that sends a single HTTP `PUT` request to a configurable endpoint when the page loads. Ideal for use as an OBS Browser Source or any other scenario where you want to trigger a remote action (e.g. in Unreal Engine) on page load, with an optional `branch` parameter passed via URL.

## Features

- **Single PUT on load** — fires once as soon as the DOM is ready.  
- **Customizable endpoint** — edit the `ENDPOINT` constant to point to your server.  
- **Dynamic branch parameter** — pass `?branch=<number>` in the URL to change the `Branch` value in the JSON payload (defaults to `0`).

## File

- `index.html` — the standalone page. Contains all logic in a single `<script>` block.

---

## Configuration

1. **Clone or download** this repo.
2. **Open** `index.html` in your favorite editor.
3. **Update** the `ENDPOINT` constant near the top of the `<script>`:
   ```js
   const ENDPOINT = 'http://127.0.0.1:30010/remote/preset/RCP_LowerThird/function/PlayAniamtion';
   ```
4. (Optional) Change the default branch by editing the fallback value in:
   ```js
   const branch = Number.isInteger(branchParam) ? branchParam : 0;
   ```

---

## Usage

1. **Serve** the page via any static server (e.g. `python -m http.server`, Nginx, GitHub Pages) Or load it off disk as an OBS source.  
2. **Load** in your browser or OBS Browser Source:
   ```
   http://<your-host>:<port>/index.html?branch=3
   ```
3. **Observe** in the browser’s Developer Tools console (F12) to see success or error logs.

---

## URL Parameters

- `branch` (integer)  
  Passed in the query string as `?branch=<number>`.  
  - If omitted or invalid, defaults to `0`.  
  - Used in the JSON payload as:
    ```json
    {
      "Parameters": { "Branch": <number> },
      "GenerateTransaction": false
    }
    ```

---

## Example

Load the page at:

```
http://localhost:8000/index.html?branch=5
```

This will send:

```json
PUT /remote/preset/RCP_LowerThird/function/PlayAniamtion
Content-Type: application/json

{
  "Parameters": { "Branch": 5 },
  "GenerateTransaction": false
}
```

---

## Deployment Tips

- **OBS**: Add as a Browser Source, point to your local/remote URL.  
- **Standalone**: Host on any static file server.  

---

## Troubleshooting

- **CORS errors**: Ensure your server allows `PUT` from your page’s origin.  
- **Network unreachable**: Verify the IP/hostname and port in `ENDPOINT`.  
- **Invalid branch**: Check that `?branch=` is followed by a valid integer.
