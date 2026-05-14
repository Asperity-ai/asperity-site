# asperity-site

One-page marketing site for **Asperity Industries, Inc.** — AI-native robotics deployment for SMB manufacturers, based in Tyler, TX.

Plain HTML + Tailwind CSS v4. No framework. No JS runtime. Hosted on Vercel.

## Stack

- **HTML** — `index.html` at root, hand-written, six sections.
- **CSS** — Tailwind v4 with custom theme tokens in `input.css`, compiled to `dist/style.css`.
- **Type** — IBM Plex Serif + Sans + Mono via Google Fonts.
- **Hosting** — Vercel (free / Hobby).

## Local dev

```sh
npm install
npm run dev    # watches input.css → dist/style.css
```

Open `index.html` directly in a browser, or use a local server:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Build

```sh
npm run build   # writes minified dist/style.css
```

Vercel runs this automatically on push via `vercel.json`.

## Edit

- **Copy** — edit `index.html`.
- **Styles** — edit `input.css` (Tailwind v4 `@theme` tokens at top, custom CSS below).
- **Photos** — `/satwant.jpg` (founder).

## Files

```
index.html         — page content
input.css          — Tailwind source + custom styles
dist/style.css     — built output (gitignored)
package.json       — tailwind CLI dependency
vercel.json        — build config + security headers + cache headers
robots.txt         — search engine directives
sitemap.xml        — single-URL sitemap
favicon.svg        — typographic mark
satwant.jpg        — founder portrait
DNS_SETUP.md       — Saturday DNS cheat sheet (one-time)
```

## Deploy

Connected to Vercel via GitHub. Push to `main` deploys to production.

Custom domains (configured in Vercel dashboard):

- `asperity.ai` (canonical)
- `asperityrobotics.com` → 301 → `asperity.ai`
- `asperityrobotics.ai` → 301 → `asperity.ai`
- `asperityai.com` → 301 → `asperity.ai`

## TODO before launch

- [ ] Generate `og-image.png` (1200×630) and place at root
- [ ] Confirm LinkedIn company page slug
- [ ] Submit `https://asperity.ai/sitemap.xml` to Google Search Console
- [ ] Submit same to Bing Webmaster Tools
- [ ] Verify all 4 domains 301 cleanly to asperity.ai
- [ ] Test schema.org JSON-LD via Google Rich Results test
