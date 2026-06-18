# CLAUDE.md — Meal Carb & Dose Helper (CFRD)

Context file for Claude Code. Read this before editing the tool.

## What this is

A single self-contained HTML web app that helps with **carbohydrate counting** for a
child with **CFRD (cystic-fibrosis-related diabetes)**. The parents use it to add up the
carbs in a meal or snack and then work out an insulin dose using the carb ratio their
diabetes team set.

- Single file: `index.html` (no build step, no dependencies, works offline). Named
  `index.html` so GitHub Pages serves it at the site root; this is also the file you edit.
- Audience: the parents (one of whom is less technical) — it must stay simple and phone-first.
- Region: South Africa. Foods, brands and units (mmol/L, grams) are SA-specific.

## What it does

- Tap items to build a "plate"; it totals the carbs (sticky bar at the bottom).
- Optional field: "1 unit covers __ g carb" → shows an estimated bolus = carbs ÷ ratio,
  rounded to the nearest ½ unit.
- A collapsible **"How this works with CFRD"** guide at the top explains the basics,
  decodes the jargon (bolus, basal, carb ratio, correction factor, hypo) and covers
  highs (hyperglycaemia) and lows (hypoglycaemia), with a red-flag box for when to seek care.
- Items are grouped: Breakfast, Mains, Sides, Syrups & toppings,
  Snacks — low carb (won't spike), Sugar-free / diabetic treats, Snacks — carb,
  Fruit, Drinks, Desserts & sweets.

## SAFETY GUARDRAILS — do not remove or weaken these

This tool is a **carb-counting aid only**. It is **not** a dosing authority.

1. **No dosing numbers beyond carbs ÷ the user's own ratio.** Never hardcode an insulin
   ratio, correction factor, or dose. The ratio is entered by the user and comes from
   their care team.
2. The tool deliberately **excludes** correction doses, insulin-on-board, illness and
   exercise. Keep the disclaimer that says so.
3. Keep the "confirm with your diabetes team" framing throughout.
4. Carb values are **estimates** — homemade dishes vary. Keep the "check the pack label /
   weigh it" guidance.
5. For sugar-free items, count the **glycaemic carb**, not the label "total carbohydrate"
   (polyols like isomalt/maltitol inflate the total). Keep notes that maltitol counts more
   than isomalt/erythritol and that polyols in quantity loosen stools (matters for CF gut).
6. Do not add child's name or any identifying info (privacy).

## Data model — how to add / edit items

All food data lives in the `DATA` array in the `<script>` block. Each group:

```js
{ group:"Group name", items:[
  { id:"uniqueId", name:"Display name", portion:"portion text", carb:NN, tag:"slow" },
]}
```

- `id` — unique short string, no spaces.
- `carb` — grams of carbohydrate for the stated `portion` (use the **glycaemic** carb for
  sugar-free items).
- `tag` (optional):
  - `"protein"` → renders a "no carb" badge (near-zero-carb foods: biltong, cheese, ham…).
  - `"slow"` → renders an "acts slower" badge (high-fat meals that raise glucose later and
    longer: chips, pizza, creamy pasta, donuts, crisps…).
- `portion` is free text; use it to carry short notes ("check label", "estimate",
  "barely raises sugar").

To add an item: drop a new object into the right group. Nothing else needs changing —
the UI, totals and dose estimate update automatically.

## Carb-value sourcing rules

- Prefer SA sources, especially **fatsecret.co.za**, then pack labels, then reputable
  nutrition databases.
- Round sensibly; note estimates in the `portion` text.
- Record the basis in commit messages if a value is non-obvious.

## Tech notes

- **No localStorage / sessionStorage** anywhere (not supported in some embeds; also keeps
  it stateless). State is in-memory only.
- PWA install metadata is **baked into the file**: apple-touch-icon, web manifest
  (both as data URIs), theme-color and apple-mobile-web-app meta tags. The app icon is a
  white glucose droplet on teal. No separate icon/manifest files are needed.
- Offline: because everything is in one file, once the page is added to the home screen it
  works without a connection.
- Keep it a single file. If you split assets, you break the "one file, works offline" property.

## Deploy: host the file → link with icon on an Android phone

Goal: host the file so it has a URL, then "Add to Home screen" on Android so it becomes an
app icon. No app store, no sideloading, no phone security changes. The file is fully
self-contained (icon + manifest baked in), so only the one file is uploaded.

### Option A: your own / your wife's web server (simplest, can stay private)

1. Ensure the server serves over **HTTPS**. This matters: over HTTPS the app opens
   full-screen/standalone and is trusted; over plain HTTP Android will still add it but it
   opens in a browser tab instead.
2. Upload the file as `index.html` into the web root or a subfolder, e.g.
   `public_html/carb/index.html` (via SFTP, cPanel File Manager, etc.).
3. The URL is then e.g. `https://yourdomain.co.za/carb/`.
4. Updating: overwrite the file; reopen on the phone once online to refresh the cache.
5. Privacy: keep the URL unlisted, or add `.htaccess` basic auth (a login prompt is a bit
   clunky for a home-screen app, and there's no sensitive data, so unlisted is usually fine).

### Option B: GitHub Pages

Put the file in the repo as `index.html`

GitHub Pages serves `index.html` at the site root, so rename the file:

```bash
git clone git@github.com:<you>/<repo>.git
cd <repo>
cp /path/to/sa-meal-carb-helper.html index.html   # served at the site root
git add index.html CLAUDE.md
git commit -m "Add CFRD meal carb & dose helper (PWA, single file)"
git push origin main
```

(Keep the original `sa-meal-carb-helper.html` too if you like — only `index.html` is what
Pages serves at root.)

#### Enable GitHub Pages

Repo → **Settings → Pages** → Source: **Deploy from a branch** → Branch: `main`,
folder `/ (root)` → Save. After a minute you get a URL like
`https://<you>.github.io/<repo>/`.

> **Private-repo caveat:** publishing Pages from a *private* repo needs **GitHub Pro**
> (personal accounts) or a paid org plan. If you're on the free tier, either upgrade,
> make this repo public (there's no sensitive data in the tool — generic SA carb tables),
> or host the single file on **Netlify Drop** / **Cloudflare Pages** instead. The published
> page is publicly viewable via its URL regardless of repo visibility.

### Add it to the Android home screen (either option)

On the phone, in **Chrome**:

1. Open the URL (web-server or Pages).
2. Tap the **⋮** menu (top right).
3. Tap **"Add to Home screen"** (may show as **"Install app"**).
4. Confirm. The glucose-droplet icon appears on the home screen and opens full-screen,
   like an app. It works offline after the first open.

### Updating later

Edit `index.html`, commit and push. Pages redeploys automatically. On the phone, reopen the
icon (Chrome may need one online open to refresh the cache); the icon and link stay the same.

## Quick maintenance checklist

- [ ] New item added to the correct `DATA` group with `id`, `name`, `portion`, `carb` (+ `tag`).
- [ ] Sugar-free item uses **glycaemic** carb, with a label note.
- [ ] Safety disclaimers and the CFRD guide left intact.
- [ ] Still a single file; PWA meta still present.
- [ ] Committed and pushed; Pages redeployed; tested on the phone.
