# northsouthlabs.co

Static splash page for North South Labs, served by GitHub Pages at
[northsouthlabs.co](https://northsouthlabs.co).

## Contents

| File | Purpose |
| --- | --- |
| `index.html` | The whole page — self-contained HTML + CSS, no build step, no JS |
| `og.png` | 1200×630 social share card |
| `favicon.svg`, `apple-touch-icon.png` | Site icons |
| `CNAME` | Custom domain for GitHub Pages — do not delete |
| `.nojekyll` | Tells Pages to serve files as-is |
| `robots.txt`, `sitemap.xml` | Basic SEO |

## Editing

Edit `index.html` and push to `main`. Pages redeploys in about a minute.

To preview locally:

    python3 -m http.server 8787

then open http://localhost:8787.

## Regenerating the social card

`og.png` is rendered from an HTML source with headless Chrome:

    "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
      --headless --disable-gpu --hide-scrollbars \
      --virtual-time-budget=6000 --window-size=1200,630 \
      --screenshot=og.png file://$PWD/og-source.html

## Origin

Compiled from the Claude Design source in
`North South Labs Landing Page/North South Labs Splash.dc.html`. The design file
depends on a React + Babel preview runtime; this is the plain static equivalent —
same layout and type, plus responsive breakpoints, focus styles, reduced-motion
support, and social metadata.

The fuller multi-section landing page (`North South Labs.dc.html`) has not been
published yet.
