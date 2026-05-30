# State-Level Welfare Stack BETA

Prototype A for the **Taxpayer-Paid Welfare Stack** project.

A searchable research prototype mapping state-administered, state-supplemented, and
state-specific public benefit layers across **New York, Vermont, and California**. The
prototype shows how state implementation records connect back to federal program
anchors and how the model can expand to all 50 states plus Washington, DC.

Live demo: https://state.timenowdata.app

## Quick start

```bash
npm install
npm run dev        # http://localhost:5000
npm run build      # production bundle into dist/
node scripts/gen_csv.mjs   # regenerate programs.csv from programs.json
```

The app is a single-page React/Vite client served by an Express shell. No
authentication, no external APIs, no private intake forms, and no localStorage. Data is
loaded from static JSON in `client/public/data/`.

## Dataset

| File | Purpose |
| --- | --- |
| `client/public/data/programs.json` | Canonical 45-record dataset (3 states × 15 program families) |
| `client/public/data/programs.csv` | Generated CSV derived from `programs.json` |
| `client/public/data/national_sources.json` | National-source catalog surfaced in the UI |

To extend, append objects to `programs.json` using the existing schema, then run
`node scripts/gen_csv.mjs`.

### Schema

```
state_name, state_abbr, region, program_family, state_program_name,
administering_agency, application_portal, source_url, federal_anchor_url,
benefit_type, target_population,
eligibility_lenses[], income_rule_summary, work_rule_summary, immigration_note,
recertification_burden, digital_access_notes, appeal_contact_notes,
advocacy_barrier_tags[], waiver_or_expansion_tags[],
verification_status, last_reviewed, notes
```

### Verification tiers

- **verified official source** — record cites a current state agency or program page.
- **national directory anchored** — anchored to a federal directory.
- **state source pending** — landscape is moving (litigation, transitions).
- **needs update** — known to be stale.

## Architecture

- `client/src/pages/Dashboard.tsx` — single-page dashboard (KPIs, search, filters,
  matrix, cards, drawer, national source catalog, methodology dialog).
- `client/src/components/Logo.tsx` — inline SVG logo (three-column civic mark).
- `client/src/lib/theme.tsx` — light/dark toggle seeded from `prefers-color-scheme`.
- `client/src/lib/types.ts` — TypeScript types for records and sources.
- `client/src/index.css` — design tokens (civic teal primary at `186 64% 32%`).
- `scripts/gen_csv.mjs` — JSON → CSV regenerator.

## Design conventions

- **Civic teal** primary (`hsl(186 64% 32%)` / `hsl(186 64% 32%)`). One accent only.
- **Inter** for body and UI, **Source Serif 4** for headings, **JetBrains Mono** for
  numbers and tags. All preloaded via the Google Fonts link in `index.html`.
- **Verification badges** use four subdued tones — teal (verified), amber, orange,
  red — never bright. See `verificationTone()` in `Dashboard.tsx`.
- **No gradients, no decorative blobs, no emoji.** Type, rule, and white space carry
  the design.
- Headings cap at `text-2xl`. Numbers use `font-mono tabular-nums`.

## Mobile

- iPad-first sidebar collapses below `lg`. Mobile uses a left-side filter sheet,
  triggered by the `Filters` button.
- Coverage matrix scrolls horizontally on narrow screens with a sticky first column.
- Cards reflow 1 / 2 / 3 columns.

## Deploy

The main agent will deploy. The static output lives in `dist/public/` after
`npm run build`. Note: production data files are copied into `dist/public/data/`
because they live in `client/public/`.

## What this is not

Research prototype only. Not legal, benefits, or financial advice. Not an eligibility
determination. Dollar amounts are intentionally omitted in favor of cited official-source
URLs. Always confirm at the source before counseling a client or making financial
decisions.
