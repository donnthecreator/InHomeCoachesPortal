# InHome Coaches Portal

Live site: https://inhomecollegeprospects.com

The coach-facing side of InHome Recruiting Intelligence. College personnel staff enter their program access code and land on a prospect board built from InHome scouting reports: board view by position, map view, and full EIS report detail for every prospect.

Built by Athlete's Inc. (The Lab USA / InHome).

## What this repo is

One file: `index.html`. The entire portal lives inside it, including all styles, all JavaScript, the Athlete's Inc. logo, and the current prospect data. There is no build step, no dependencies to install, and no other files required.

If you are looking at this repo months from now and wondering where the rest of the code is: there is no rest. One file is the whole site.

## How it works

- **Gate screen.** Visitor enters a program access code. Each code maps to a program name shown on their cover bar. Codes live in the `PROGRAMS` object near the top of the script. The master list of codes is kept privately, not in this repo.
- **Board view.** Prospects grouped into position columns, ranked by InHome Score. Coaches can drag cards to set their own order; that reordering is saved in their browser only and never changes the underlying score. A banner appears with a reset button when a custom order is active.
- **Map view.** Tries Google Maps first, falls back to Leaflet/OpenStreetMap if Google is blocked, and falls back again to a built-in offline map if all outside servers are unreachable. A coach always sees pins, never a blank box. Pins carry the prospect popup and a Full Report button.
- **Report modal.** Full EIS scouting report: film traits, athletic and production grades, character gates, standalone Football IQ gauge, scout narrative, verified track marks when present, and the recommendation tier.
- **Scoring.** InHome Score composite is Film 50%, Athletic 25%, Character 15%, Production 10%. Football IQ is a separate 1 to 10 gauge, not part of the composite. Scores are computed in the page from the stored grades.

## Where the data comes from

Right now the prospect reports are embedded in the `REPORTS` array inside `index.html`.

The portal is also wired for live data. Near the top of the script there is a `LIVE_URL` constant. When it is set to the `list-reports` Netlify function on the builder site, the portal fetches reports from the Neon database on every load and falls back to the embedded board if the fetch fails. Until then, `LIVE_URL` stays `null` and the embedded board is what coaches see.

The write side (the scout report builder and the database functions) lives in a separate repo and a separate Netlify site. This repo never touches the database directly and contains no database credentials.

## How to update the site

1. Get the new `index.html`.
2. In this repo on github.com: Add file, then Upload files.
3. Drag the new `index.html` on top of the old one and commit.
4. Netlify sees the commit and redeploys automatically, usually under a minute.

To roll back a bad deploy: Netlify, Deploys tab, pick the previous deploy, Publish deploy.

## Things to keep straight

- The homepage must be named exactly `index.html`.
- Access codes are uppercase in the config; the gate uppercases whatever the coach types, so codes are effectively case-insensitive for them.
- The Google Maps API key in the file is restricted by HTTP referrer to this site's domain, so it is not usable from anywhere else.
- Keep this repo private. The access codes inside `index.html` are the only thing standing between the public and each program's board.
