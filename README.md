# cpurick.com — static export

This is a static snapshot of cpurick.com (originally built on Ghost), cleaned up
and ready to host on GitHub Pages. It's plain HTML/CSS/JS — no build step, no
Jekyll required.

## What's here / what changed from the raw mirror

- Removed a `cdn-cgi/` folder of decoy pages — these were Cloudflare's
  AI-scraper honeypot content, not part of the real site.
- Removed a hidden Cloudflare bot-management script snippet (irrelevant
  without Cloudflare in front of the site).
- Renamed cache-busted asset files (e.g. `screen.css?v=xxxx.css` →
  `screen.css`) and fixed every reference to them.
- Removed the Ghost "Portal" (subscribe popup) and "Sodo Search" scripts,
  since they call a live Ghost API (`/ghost/api/content/`) that won't exist
  once this is static. They'd have failed silently anyway, but this is
  cleaner.
- Added `.nojekyll` so GitHub Pages serves the files as-is instead of running
  them through Jekyll.
- Added `CNAME` with `cpurick.com` for the custom domain.

## What this does NOT include

This is a frozen snapshot, not a live blog:
- No subscribe/membership popup, no search box (both needed a live Ghost
  backend).
- No comments.
- New posts you publish elsewhere won't show up here — you'd need to
  re-export and re-push (see below).

## Set it up on GitHub Pages

1. **Create a new repo** on GitHub (e.g. `cpurick-site`). It can be public or
   private (Pages works either way on paid plans; public repos get free
   custom-domain Pages).
2. **Push these files to the repo root** (not a subfolder):
   ```bash
   cd path/to/this/folder
   git init
   git add -A
   git commit -m "Static export of cpurick.com"
   git branch -M main
   git remote add origin https://github.com/<you>/cpurick-site.git
   git push -u origin main
   ```
3. In the repo, go to **Settings → Pages**.
   - Source: "Deploy from a branch"
   - Branch: `main`, folder: `/ (root)`
   - Save.
4. GitHub will give you a URL like `https://<you>.github.io/cpurick-site/`.
   Confirm the site loads and looks right there first.
5. **Point your custom domain (cpurick.com) at GitHub Pages:**
   - In your DNS provider, set up either:
     - An **ALIAS/ANAME record** (or `A` records pointing at GitHub's IPs:
       `185.199.108.153`, `185.199.109.153`, `185.199.110.153`,
       `185.199.111.153`) for the apex domain `cpurick.com`, **or**
     - A **CNAME record** for `www.cpurick.com` → `<you>.github.io`
   - Back in **Settings → Pages**, under "Custom domain", enter `cpurick.com`
     and save (this repo already has the `CNAME` file, so GitHub should
     detect it automatically). Wait for DNS check to go green, then enable
     "Enforce HTTPS".
6. **Turn off/redirect your old Grav or Ghost host** once DNS has propagated
   and you've confirmed GitHub Pages is serving correctly.

## Updating content later

Since this is a static snapshot, adding a new post means:
1. Publish/edit on your current CMS as usual (if you're keeping it running
   somewhere, even privately) — or hand-write a new HTML page based on an
   existing post as a template.
2. Re-run the mirror/export process, or manually add the new page + update
   any listing pages (`index.html`, `tag/projects/index.html`, `rss/index.html`)
   that should reference it.
3. Commit and push — GitHub Pages redeploys automatically within a minute or
   two.
