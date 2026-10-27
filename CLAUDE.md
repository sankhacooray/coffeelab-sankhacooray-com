# Coffee Lab — coffeelab.sankhacooray.com

Single-file HTML5 app (`index.html`, no build, no framework). GitHub Pages: pushing `main` deploys.
Follows the `ui-ux-style` skill (PRINCIPLES.md), especially rules 33/36 (nothing scrolls, page instead)
and 37 (low cognitive load: phases, visual forms, details behind ⓘ).

## Layout
- App bar: brand · phases **Learn | Make | Taste** · search · settings gear · theme switch · CTA "Pour a random coffee".
  Internal phase ids are `pick` / `build` / `taste`.
- Left pane (`.cupside`): verdict (fixed height so the cup never moves) · cup scene · tabs pinned at the bottom
  **Ingredients | Brew guide | Nutrition** (`.cup-views .cv`, one at a time). Narrow screens (<900px): two-column grid,
  cup in the right column scaled with CSS `zoom` (`sizeCupColumn`), short tab labels.
- Right pane: Learn = family pills + drink tiles · Make = 7 stage tabs + ingredient tiles · Taste = Flavour radar |
  Flavour bars (draggable target profile) + "Inside your profile" suggestions.
- Settings = full view (`#settings`, `body.on-settings`), opened by the gear; Units + Blend names.

## Data (treat as data — don't change without asking)
`INGREDIENTS`, `STAGES`, `COFFEES` (recipe = parts, 1 part ≈ 30 ml). Derived tables added later, all estimates:
`CUPS` (sizes), `SERVE`/`cupTemp`/`prepTemp` (°C), `TASTE` + `AXES` (WCR lexicon axes, sources in the Taste ⓘ),
`KCAL`, `NUTRI` (USDA/labels; EFSA caffeine, WHO sugar guides), `NAME_SLOTS` + `blendName()` (systematic names
for custom cups: `[Iced] [Spiked] [Flavour] [Milk] [Shots] Base [Style] [Topping]`).
Invariants: no preset is ever flagged by `clash()` against its own ingredients; `blendName()` never equals a famous
coffee for a cup that isn't one.

## No-scroll machinery
- `fitPages(root, pager, key)` pages grid tiles (drinks, ingredients, matches); `data-stretch` grows rows to fill.
- `fitCupView()` pages the left-pane lists (total/serve rows pinned; nutrition folds warnings into "+N more").
- ResizeObservers re-fit on size changes; `#scn-footer` reserves the shared footer's final height (56/52px).

## Storage
`localStorage["coffeelab-settings"]` = `{temp, vol, energy, chart, target}`; theme uses the network-wide `sc-theme`.
The settings view warns that clearing site data resets them.

## Deploy checklist
1. Bump `CACHE` in `sw.js` (`coffeelab-vN`) on every release, or installed PWAs keep the old page.
2. Commit + push `main`, then `curl -s "https://coffeelab.sankhacooray.com/?nc=$RANDOM"` until the new markup shows.
3. Before pushing, verify with headless Chrome iframes at 1352×690, 1280×640, 1920×1080, 820×1180, 500×900,
   390×844, 375×667, 360×740: no page scroll, no `.cv`/phase overflow, no cut tiles. Disable transitions in
   test frames (`*{transition:none!important}`) — headless virtual time freezes them and fakes bugs.
   Never bind a local test server inside 4710–4779 (reserved); 8743 has been used.
