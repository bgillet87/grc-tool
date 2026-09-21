# Control Register

A self-contained GRC toolkit: a multi-framework control register (ISO 27001,
PIPEDA, NIST Privacy Framework, ITSG-33) with blended-audit mode, a document
builder for policies/risk registers/SoAs/PIAs, and an audit-method field guide.

Everything lives in `index.html` — no build step, no backend, no dependencies
to install. Data is stored in the browser's local storage, per device.

## Files

- `index.html` — the entire application
- `manifest.json` — makes it installable as a desktop/home-screen app
- `sw.js` — service worker for basic offline use
- `icons/`, `apple-touch-icon.png` — app icons

## Deploy it (pick one — all are free)

### Option A: GitHub Pages
1. Create a new GitHub repository (public or private, both work with Pages
   on a paid plan; public repos get Pages free).
2. Upload all the files in this folder to the repo (drag-and-drop on
   github.com works, or `git add . && git commit -m "control register" && git push`).
3. In the repo, go to **Settings → Pages**.
4. Under "Build and deployment", set **Source** to "Deploy from a branch",
   pick your main branch and the `/ (root)` folder, then **Save**.
5. GitHub gives you a URL like `https://yourusername.github.io/repo-name/`
   within a minute or two.

### Option B: Netlify (fastest — no account needed to try it)
1. Go to **app.netlify.com/drop**.
2. Drag this whole folder onto the page.
3. Netlify hosts it instantly at a random `*.netlify.app` URL. Create a free
   account afterward if you want to keep it and get a custom subdomain.

### Option C: Cloudflare Pages
1. Go to **pages.cloudflare.com**, sign in, "Create a project" → "Upload assets".
2. Drag this folder in. You get a `*.pages.dev` URL.

Any of these work equally well — pick whichever you're most comfortable
maintaining. GitHub Pages is the natural choice if you want the source
sitting in a repo (handy for a portfolio link during interviews).

## "Installing" it as a desktop app

Once it's hosted on a real HTTPS URL (any of the above), open it in
**Chrome or Edge** on your desktop:

1. Look for an install icon (a monitor with a down-arrow, or a "+") at the
   right side of the address bar.
2. Click it → **Install**.
3. It now opens in its own window, with its own icon in your taskbar/dock —
   no browser chrome, no tabs. This is a real installed web app (a PWA),
   not a bookmark.

Firefox doesn't support this install flow; Safari on macOS has limited
support. Chrome, Edge, and Chromium-based browsers (including Vanadium on
GrapheneOS) handle it well.

## Before you deploy — quick checklist

- **Test locally with a real server, not double-clicking the file.** Service
  workers refuse to register over `file://`. From inside this folder, run
  `python3 -m http.server 8000` and open `http://localhost:8000` to test the
  real behaviour (install prompt, offline reload) before pushing live.
- **Redeploys show up immediately.** The service worker fetches `index.html`
  fresh from the network whenever you're online and only falls back to the
  cached copy if you're offline — so you won't get stuck seeing an old
  version after you update and re-push. If you ever rename or remove one of
  the files listed in `APP_SHELL` inside `sw.js`, bump `CACHE_NAME` (e.g.
  `v1` → `v2`) so old devices clear out the stale reference.
- **Your data never leaves your browser, even though the code is public.**
  Making the repo public only exposes the blank tool itself — nothing you
  type into the Register or Builder is transmitted anywhere or committed to
  the repo; it's pure browser local storage. So a public GitHub repo is
  fine from a data-exposure standpoint.
- **Back up before you rely on it.** Use the "Download backup" button in the
  app's footer regularly (it exports every framework's statuses, every
  saved document draft, and your Field Guide progress into one JSON file).
  "Restore backup" loads that file back in — handy for moving data to a
  new machine, or recovering from a cleared browser profile. There is no
  server copy of your data anywhere, so this file is the only backup that
  exists.
- **Think twice before logging real employer findings.** This is a great
  tool for practice, coursework, and a household/personal-project audit you
  can show off. If you're tempted to log actual, specific compliance gaps
  at your current employer here, pause first — that's organizational data
  living outside any sanctioned system, and raising real findings usually
  belongs in whatever official process your employer has for it, not a
  personal side project, however well-built.

## Cross-device sync (there isn't any, yet)

Because everything saves to browser local storage, your data is tied to
**one browser on one device**. It will not sync between your work machine
and anything else, and clearing browser data will erase it. Backups (above)
are the manual workaround. If you want real cross-device sync later, that
requires adding a small backend (e.g. a free tier on Supabase or Firebase)
— worth doing once you're using this for real audits rather than practice.

## Going further: a native desktop build (optional)

If you eventually want a genuine standalone binary (a `.exe`, `.AppImage`,
or `.dmg` you can run with no browser at all), the standard route is
wrapping this same `index.html` in **Electron**. That's a heavier lift —
it needs Node.js and produces a much larger install package — so it's
only worth it if browser-based installation (above) doesn't feel like a
"real app" enough for what you want. Ask if you'd like that scaffolded
out; it's a different project structure than this one.
