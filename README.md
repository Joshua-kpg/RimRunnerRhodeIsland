# RimRunnerRhodeIsland

Unofficial Rhode Island HS boys basketball ratings. Not affiliated with the RIIL or MaxPreps.
Status: **HELD (waiting on hosting)**. Preview: https://joshua-kpg.github.io/RimRunnerRhodeIsland/ (GitHub Pages). No Firebase project yet
(the Google accounts are at their project quota) and Cloudflare Pages is at its project limit, so accounts, picks and follows save only in the
visitor's browser until the site's own Firebase project exists.

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's future Firebase project (accounts, picks, follows).

## Teams
49 teams: the RIIL (Rhode Island Interscholastic League) Basketball Sport Handbook, Winter 2026-27 (boys alignment approved 06/15/26):
Division 1 16, Division 2 16, Division 3 17. The RIIL boys use three divisions, and each division is also the conference shown on the site.
The Pawtucket Co-op (Shea + Tolman) is one team; MaxPreps files it under Tolman's page and Shea's page is folded in. Shea and Tolman played as
separate teams in 2025-26, so the co-op starts from the Division 1 prior with no last-year history.
No Other (non-RIIL) programs met the 8-game rule.

## Schedule
2026-27 games come from MaxPreps team schedules plus the official RIIL 2026-27 boys league schedule (dated 8/6/2026). A league game from the
RIIL schedule is added only when MaxPreps has no game for that pair within 3 days; those rows have no tip time. Where the two disagree, the
MaxPreps row is kept.

## Season rule
RIIL 2026-27 boys basketball: start Mon Nov 30, 2026; games can begin Thu Dec 10, 2026 (all divisions); last regular-season day Fri Feb 12,
2027; championships concluded by Sun Mar 14, 2027.
Games involving an RIIL team before Dec 10, 2026 are left out (schedule build and nightly updater); games between two non-RIIL schools are kept.
Duplicate next-day listings of one game are removed (shared tournament-repeat rule, tested on both seasons). Game times are Eastern.

## Nightly scores
`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 06:30 and 14:00 UTC (1:30 am / 9:00 am Eastern standard time) and
on manual dispatch. Before `SEASON_START` in index.html (Thu Dec 10, 2026) the script makes no requests. From then on it reads boys basketball
finals for the last 3 days from the MaxPreps Rhode Island scores-by-date page (team-page fallback), matches them to the schedule by MaxPreps team
id + date (the co-op also by Shea's page id in `mpAliasIds`), records them in `data/results.json` and bakes scores and W-L records into
`index.html`. The page recomputes Elo from those finals. It commits only when data changed; GitHub Pages updates from the commit.
