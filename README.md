# Menu al nido

A single-page, mobile-friendly lookup for the Istituto degli Innocenti nursery
menu (fascia 1–3 anni). No build step, no dependencies, no backend — it's one
static HTML file with the menu data embedded and the current day computed
in the browser.

## Files

- `index.html` — the whole app.
- `vercel.json` — optional, only used if you deploy with Vercel.

## Run it locally

No install needed. From this folder:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

or just double-click `index.html` to open it directly in a browser.

## Deploy

Pick whichever you already use — all three are a single command against this
folder as-is.

**Vercel**
```bash
npx vercel --prod
```

**Netlify**
```bash
npx netlify deploy --prod --dir .
```

**GitHub Pages**
```bash
git init
git add .
git commit -m "Menu al nido"
git branch -M main
git remote add origin <your-empty-github-repo-url>
git push -u origin main
# then in the repo settings, enable Pages on the main branch, root folder
```

Any of these gives you a public URL you can save to your phone's home screen
(it's set up to run full-screen like an app when you do — "Add to Home
Screen" in Safari or Chrome).

## What it does

- Shows today's pranzo, spuntino, and merenda for the 1–3 anni menu, with
  the rest of the current week listed below.
- Cycles through the nursery's 4-week rotation and 4 seasons automatically,
  based on the current date.
- The rotation week and season are editable under "Week and season" (bottom
  of the page) and saved in the browser's local storage, so you only set
  them once — against whatever week the nursery is actually on.

## Extending it

The menu data lives in the `MENU` object near the top of the `<script>`
block in `index.html`, keyed by season → week (0–3) → weekday (0=Mon..4=Fri),
with `p` (pranzo, an array of dishes) and `m` (merenda, a string). Allergen
numbers are inlined in each dish string after a `|`, e.g. `"Pane|1"`.

To add the 9–12 mesi and 6–9 mesi bands from the source PDF:
1. Add two more top-level keys to a new `MENUS` object (one per age band),
   each shaped like the existing `MENU` object.
2. Add an age-band `<select>` in the header, same pattern as the season
   selector, persisted to `localStorage` the same way.
3. Swap `MENU` for `MENUS[currentBand]` in `cell()`.

The source PDF (`menu__dei_servizi_educativi.pdf`) has the 9–12 mesi and
6–9 mesi tables on the pages following the 1–3 anni ones, in the same
weekly-grid layout.
