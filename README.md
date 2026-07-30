# jacobhoward.co.uk

Personal academic site. Static HTML and one stylesheet — no build step, no
dependencies, no JavaScript. Deployed as an assets-only Cloudflare Worker.

## Structure

```
index.html          About / landing page
research/           Working paper + earlier work
cv/                 Education, appointments, outreach, technical
404.html            Not-found page
assets/style.css    The entire stylesheet
assets/papers/      PDFs go here (empty until the paper is published)
_headers            Security + cache headers (parsed by Cloudflare, not served)
_redirects          Trailing-slash normalisation (likewise)
wrangler.jsonc      Cloudflare config: assets-only Worker, 404 handling
.assetsignore       Repo files that must not be served as part of the site
sitemap.xml         Update when pages are added
```

## Editing

Open the file on github.com, click the pencil icon, edit, commit. Cloudflare
redeploys within a minute.

To preview locally first:

```bash
cd jacobhoward-site
python3 -m http.server 8000     # then open http://localhost:8000
```

Opening `index.html` by double-clicking works for reading the text, but the nav
links need a server to resolve.

## Deploying

Cloudflare Workers Builds watches `main` and runs `npx wrangler deploy`, which
reads `wrangler.jsonc`. There is no build command — leave that field empty in
the dashboard; anything in it will fail the deploy.

`wrangler.jsonc` declares an assets-only Worker (no `main`, so no server code)
serving the repo root, with `not_found_handling: "404-page"` so unmatched URLs
get `404.html` with a real 404 status.

Anything added to the repo is served unless it's listed in `.assetsignore` —
so if you add a working file you don't want public, put it there.

## Publishing the paper

1. Put the PDF in `assets/papers/`, e.g.
   `testing-piketty-in-the-long-run-2026.pdf`
2. In `research/index.html`, replace the "Request the current draft" link with a
   link to the PDF and update `<span class="tag">Working paper</span>`
3. Do the same on `index.html`
4. Add the PDF to `sitemap.xml`

## Editing conventions

- Every page repeats the masthead and nav inline. There are four pages; a
  templating system would cost more than it saves. Change the nav in
  `index.html`, `research/index.html`, `cv/index.html` and `404.html`.
- The current page in the nav carries `aria-current="page"`.
- Colours, spacing and text measure are CSS custom properties at the top of
  `assets/style.css`. Dark mode derives from them and follows the OS setting.
- `--measure` controls line length. Past ~36rem it hurts readability.

## Licence

Site code: MIT. Written content and PDFs: all rights reserved. See `LICENSE`.
