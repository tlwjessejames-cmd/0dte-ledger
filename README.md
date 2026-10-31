# The 0DTE Ledger: publishing workflow

Static site, no build step. Files: `index.html` and `styles.css`. Publish by committing both files to the GitHub Pages repository. The commit history is the timestamp proof, so never rewrite or force-push history, and never edit a graded row in place.

Source of truth for every number: `~/workspace/goals/0dte-qqq-project/files/MODEL-RULES.md` (frozen rules v1.2) and `~/workspace/goals/0dte-qqq-project/files/0dte-qqq-tracker.xlsx` (Picks and Record sheets). The site copies the tracker. It never originates a figure.

## Morning update, before the 06:30 PT open

1. After the morning job has appended the day's suggestion to the tracker, add one new row at the TOP of the day-by-day table body in `index.html` (newest first).
2. Fill only: Date, Contract (QQQ, strike, Call or Put, expiry, which is always that same day), Target.
3. Set Result to `PENDING` and leave QQQ range / close, Entry, Peak, Expiration, Max return, Expiry return, and Shadow result blank or marked `-` until grading.
4. Commit with a message naming the date and contract, for example: `Day 8 pick: 2026-11-02 QQQ 700 Put, target 690`. A suggestion only counts if it was delivered before the open, and the commit time is the public evidence of that.

## Afternoon update, after the 13:21 PT autopsy

1. Take every figure from the graded tracker row and the autopsy note. Do not recompute returns by hand.
2. Fill the pending row:
   - QQQ range / close: session low to session high, then the close, regular session only.
   - Result: `WIN` if QQQ touched the target intraday, otherwise `LOSS`.
   - Entry: the contract's opening print. This is the v1.1 grading basis.
   - Peak: the contract's intraday high.
   - Expiration: the contract's value at expiry (intrinsic value at the close).
   - Max return: Peak against Entry, as recorded in the tracker.
   - Expiry return: Expiration against Entry, as recorded in the tracker.
   - Shadow result: exit trigger, exit time PT, exit price, and shadow return, from the shadow ledger. Label it retro if it was ever computed after the fact.
3. Update the summary lines in the Record section from the tracker Record sheet: days, wins, losses, win rate, average max return, average expiry return. Then update the shadow box separately from the shadow ledger: shadow days, target-touch exits, stop-outs, expiry exits, shadow wins, average shadow return. Keep the two tracks in their separate blocks.
4. If a past figure was wrong, do not overwrite the row silently. Add a dated correction note below the table and commit it as its own commit explaining the change.
5. Commit with a message naming the date and result, for example: `Day 8 graded: 2026-11-02 WIN, expiry +80.0%, shadow -24.0%`.

## Standing rules

- One row per market weekday. If US equities are closed, no row and no post.
- Losses get the same format and prominence as wins. No row is ever deleted.
- Entry, Peak, and Expiration are always the three price columns, in that order, with exactly those labels.
- No language beyond the frozen format: no commentary, no forecasts, and no advice anywhere on the page. The autopsy reasoning stays in the project files, not on the site.
- Rules text on the page changes only when MODEL-RULES.md changes at a scheduled review. Next review: Thu 2026-12-17.
