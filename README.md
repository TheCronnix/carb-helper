# Meal Carb & Dose Helper (CFRD)

A simple, phone-first web app for **carbohydrate counting** for a child with
**cystic-fibrosis-related diabetes (CFRD)**. Add up the carbs in a meal or snack,
then use your own carb ratio to work out a dose.

South African foods, brands and units. One self-contained file — no install, works offline.

## Using it

1. Tap **+ / −** next to foods to build the plate. The total carbs show at the bottom.
2. In the bottom bar, type your child's carb ratio in **"1 unit covers __ g carb"**.
   It shows an estimated dose = carbs ÷ ratio, rounded to the nearest ½ unit.
3. Tap **"How this works with CFRD"** at the top for a plain-language guide
   (carb counting, the jargon, and what highs and lows mean).
4. **Clear plate** resets it.

Foods are grouped: Breakfast, Mains, Sides, Syrups & toppings,
Snacks — low carb (won't spike), Sugar-free / diabetic treats, Snacks — carb,
Fruit, Drinks, Desserts & sweets.

> Tags: **"no carb"** = barely needs insulin (biltong, cheese, ham…).
> **"acts slower"** = high-fat meals that raise glucose later and longer (chips, pizza,
> creamy pasta, donuts…).

## Important

This is a **carb-counting aid only — not a dosing authority.** The estimate is just
carbs ÷ your ratio. It does **not** include corrections for a high reading, insulin still
active from a previous dose, illness or exercise. Use the ratios and rules your diabetes
team set, and confirm doses with them. Carb values are estimates — check the pack label or
weigh the food when you can. For sugar-free items, count the **glycaemic** carb on the
label, not the "total carbohydrate".

## Putting it on a phone (no app store needed)

The file just needs to live at a web link, then you "Add to Home screen". Pick one:

**Your own web server (simplest, can stay private)**
1. Make sure the server uses **HTTPS** (so it opens full-screen and is trusted).
2. Upload the file as `index.html` into a folder, e.g. `public_html/carb/index.html`.
3. The link is then `https://yourdomain.co.za/carb/`.

**Free hosting**
- **Netlify Drop** (app.netlify.com/drop) — drag the file on, get a link instantly.
- **GitHub Pages** — commit `index.html`, enable Pages. (Private repos need GitHub Pro;
  otherwise make the repo public — no sensitive data here — or use Netlify.)

**Then, on Android (Chrome):**
Open the link → **⋮** menu → **Add to Home screen** → confirm.
The droplet icon appears and opens full-screen. Works offline after the first open.

**On iPhone (Safari):**
Open the link → **Share** → **Add to Home Screen** → Add.

To update later, re-upload / re-push the file. The icon and link stay the same; reopen it
once online to refresh.

## Files

- `index.html` (or `sa-meal-carb-helper.html`) — the app. Single file, self-contained.
- `CLAUDE.md` — context and editing notes for Claude Code.
- `README.md` — this file.
