# northsouthlabs.co

Static splash page for North South Labs, served by Vercel at
[northsouthlabs.co](https://northsouthlabs.co). No build step, no framework, no JS.

This project also owns the apex routing, so it is the reason
[northsouthlabs.co/ai/archive](https://northsouthlabs.co/ai/archive) resolves — see *Routing*
below before changing `vercel.json`.

## Contents

| File | Purpose |
| --- | --- |
| `index.html` | The whole page — self-contained HTML + CSS |
| `vercel.json` | Routing, headers, and the rewrite that serves the Archive under `/ai/archive` |
| `og.png` | 1200×630 social share card |
| `og-source.html` | The HTML `og.png` is rendered from |
| `favicon.svg`, `apple-touch-icon.png` | Site icons |
| `robots.txt`, `sitemap.xml` | Basic SEO |
| `.nojekyll` | Vestigial, from the GitHub Pages era. Harmless; Vercel does not run Jekyll |

## Deploying

The Vercel project `northsouthlabs-co` is connected to this repo, so **a push to `main` deploys
to production**. There is nothing to run by hand.

It was CLI-only (`vercel --prod`) until 2026-08-22, which is worth knowing because the failure
mode of a manual deploy is silence — the sibling Archive project sat a day behind five merged
commits without anything indicating it.

If the connection ever needs redoing, note that `vercel git connect` reads the local `origin`
and cannot parse this repo's SSH alias (`git@github-nslabs:…`, which selects the right key for
the nslabs account); it also ignores a URL passed as an argument. Point `origin` at
`https://github.com/allen-nslabs/northsouthlabs.co.git` for the length of the connect, then
restore the alias — pushing depends on it.

## Routing

`vercel.json` rewrites `/ai/archive` and `/ai/archive/:path*` to the `ai-archive` Vercel
project, which is a separate repo. Two consequences worth remembering:

- **The apex and the Archive share an origin.** That was chosen deliberately over a subdomain,
  and it means the Archive's Supabase session in `localStorage` is reachable by any script on
  this page. Keep this page dependency-free; it is not just a stylistic preference.
- **Headers set here apply to the rewritten path too.** `Referrer-Policy:
  strict-origin-when-cross-origin` overrides the stricter `same-origin` the Archive asks for.
  That is fine — `strict-origin` already withholds the path cross-origin, and the identifying
  part of an Archive URL is in the hash fragment, which browsers never send in a referrer — but
  it is the kind of thing to check before adding a header here.

## Editing

Edit `index.html` and push to `main`. To preview locally:

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
`North South Labs Landing Page/North South Labs Splash.dc.html`. The design file depends on a
React + Babel preview runtime; this is the plain static equivalent — same layout and type, plus
responsive breakpoints, focus styles, reduced-motion support, and social metadata.

The fuller multi-section landing page (`North South Labs.dc.html`) has not been published yet.
