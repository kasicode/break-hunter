# Break Check

Photograph a record sleeve, and it's checked against 2,140 titles with a
known open drum break — matched by artist and title, read straight off
the photo with on-device OCR. No backend, no database: everything runs
in the browser, including the OCR engine, which is self-hosted in this
repo (`/tesseract`) instead of pulled from a CDN.

## What's in here

- `index.html` — the whole app: UI, matching logic, and the embedded
  2,140-record dataset.
- `tesseract/` — a self-hosted copy of the Tesseract.js OCR engine
  (script, WASM core, English language data), so recognition doesn't
  depend on any third-party CDN being reachable.
- `manifest.json` + `icon-*.png` — makes "Add to Home Screen" produce a
  real standalone app icon instead of a browser bookmark.
- `package.json` — a one-line static file server so Railway (or any
  Node host) knows how to serve this.

## Deploy it yourself

### 1. Get this into a GitHub repo

- Create a new repository on GitHub (empty, no README/license needed
  since one's already here).
- From this folder:
  ```
  git init
  git add .
  git commit -m "Break Check"
  git branch -M main
  git remote add origin https://github.com/<your-username>/<repo-name>.git
  git push -u origin main
  ```

### 2. Deploy on Railway

- At [railway.com](https://railway.com), **New Project → Deploy from
  GitHub repo**, and pick the repo you just pushed.
- Railway detects the `package.json` automatically, runs `npm install`
  then `npm start`, and serves the app.
- Once it's live, go to the service's **Settings → Networking** and
  generate a public domain — that's the URL to open on your phone.

### 3. Add it to your home screen

Open that Railway URL in Safari or Chrome, then use the browser's own
"Add to Home Screen" (Safari: Share icon; Chrome: ⋮ menu). Because this
version ships a real `manifest.json`, it should install as a proper
standalone app icon rather than just a bookmark.

## Notes

- Everything runs client-side — the photo, the OCR, and the matching
  never leave the phone, and nothing is sent to a server.
- The `tesseract/` folder is ~18 MB (mostly the English language
  model). That's normal — it's what makes recognition work without
  depending on an outside CDN being reachable.
