# Smart Car Shortlister

A tiny full-stack MVP that turns a budget + a few preferences into a ranked
shortlist of 3-5 cars. Built with Next.js (App Router) + TypeScript, using a
local JSON file as the "database" — no external services required.

## Run it

```bash
npm install
npm run dev
```

Then open http://localhost:3000.

## How it works

- **`data/cars.json`** — the car catalog (15 cars: name, price, mileage,
  safety rating, fuel type, body type, suitable usage).
- **`lib/recommend.ts`** — all recommendation logic, kept separate from the
  API route and UI:
  1. Filters cars by budget, fuel type, and usage.
  2. Scores each remaining car 0–1 using a weighted blend of mileage,
     safety rating, and price fit.
  3. The user's chosen priority (Mileage / Safety / Performance) shifts the
     weights toward what they care about most.
  4. Sorts by score and returns the top matches (max 5).
- **`app/api/recommend/route.ts`** — `POST /api/recommend`. Validates the
  request body and delegates scoring to `lib/recommend.ts`.
- **`app/page.tsx`** + **`app/components/`** — the form, loading state, and
  result cards. A plain `fetch` call to the API route on submit.

## Project structure

```
app/
  api/recommend/route.ts   # API route (POST)
  components/CarForm.tsx   # Preference form
  components/ResultsList.tsx  # Result cards
  page.tsx                 # Page shell, fetch + state
  globals.css
data/cars.json             # "Database"
lib/
  recommend.ts             # Filtering + scoring logic
  types.ts                 # Shared TypeScript types
```

## Notes

This is an MVP: the scoring is a simple weighted formula, not a machine
learning model, and the dataset is static. Both are intentionally easy to
extend — add more cars to `cars.json`, or tweak the weights in
`lib/recommend.ts`.
