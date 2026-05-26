# coffeelab.sankhacooray.com

A tiny visual lab for building coffee drinks. Pick a famous coffee, or add
and remove ingredients in a virtual cup and watch which famous drink you
just assembled.

Single-file HTML5 app — no build step, no framework, installable as a PWA.

## Run locally

```
python3 -m http.server 8000
# open http://localhost:8000
```

## Deploy

Pushed to `main` on https://github.com/sankhacooray/coffeelab-sankhacooray-com
and served by GitHub Pages with the CNAME `coffeelab.sankhacooray.com`
(DNS via Cloudflare on the `sankhacooray.com` zone).

## Add a coffee

Edit `COFFEES` in `index.html`. Each row is
`{ id, name, family, recipe: { <ingredient>: <parts> } }`. Parts are
30 ml units. A drink matches when the cup's ingredient mix equals the
recipe exactly.

## Add an ingredient

Edit `INGREDIENTS` in `index.html`. Each entry needs a `color` and a
`density` rank — lower density sinks to the bottom of the cup, higher
floats on top. Optional `klass` lets you apply a textured layer style
(`foam`, `whipped`, `ice`, `cocoa`, `sugar`, `icecream`).
