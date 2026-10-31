# The 0DTE Ledger: publishing workflow

Static site, no build step. Files: `index.html` and `styles.css`. Publish by committing both files to the GitHub Pages repository. The commit history is the timestamp proof, so never rewrite or force-push history, and never edit a graded row in place.

Source of truth for every number: `~/workspace/goals/0dte-qqq-project/files/MODEL-RULES.md` (frozen rules v1.2) and `~/workspace/goals/0dte-qqq-project/files/0dte-qqq-tracker.xlsx` (Picks and Record sheets). The site copies the tracker. It never originates a figure.

## Morning update, before the 06:30 PT open

1. After the morning job has appended the day's suggestion to the tracker, add one new row at the TOP of the day-by-day table body in `index.html` (newest first).
2. Fill only: Date, Contract (QQQ, strike, Call or Put, expiry, which is always that same day), Target.
3. Set the Result cell to `<span class="pill pending">PENDING</span>` and leave QQQ range / close, Entry, Peak, Expiration, Max return, Expiry return, and Shadow result blank or marked `-` until grading.
4. Commit with a message naming the date and contract, for example: `Day 8 pick: 2026-11-02 QQQ 700 Put, target 690`. A suggestion only counts if it was delivered before the open, and the commit time is the public evidence of that.

## Afternoon update, after the 13:21 PT autopsy

1. Take every figure from the graded tracker row and the autopsy note. Do not recompute returns by hand.
2. Fill the pending row:
   - QQQ range / close: session low to session high, then the close, regular session only.
   - Result: `<span class="pill win">WIN</span>` if QQQ touched the target intraday, otherwise `<span class="pill loss">LOSS</span>`. The Result cell always uses exactly this pill markup.
   - Entry: the contract's opening print. This is the v1.1 grading basis.
   - Peak: the contract's intraday high.
   - Expiration: the contract's value at expiry (intrinsic value at the close).
   - Max return: Peak against Entry, as recorded in the tracker.
   - Expiry return: Expiration against Entry, as recorded in the tracker.
   - Shadow result: exit trigger, exit time PT, exit price, and shadow return, from the shadow ledger. Label it retro if it was ever computed after the fact.
3. Update the Record section from the tracker Record sheet. The summary figures live in the record banner: the six `.stat-value` values, in order: days graded, wins, losses, win rate, average max return, average expiry return. The `.banner-dates` line carries the first and last graded dates, in the format `Thu 22 Oct 2026 to Fri 30 Oct 2026`. Then update the shadow box separately from the shadow ledger: shadow days, target-touch exits, stop-outs, expiry exits, shadow wins, average shadow return. Keep the two tracks in their separate blocks.

   The graded-day count appears in exactly five places on the page. Every one of them is a machine-updated element, every one is updated at every grading from the tracker, and if any one disagrees with the tracker the page is wrong:
   - the day-by-day table itself: one row per graded day;
   - the `.banner-dates` line in the record banner: the first and last graded dates;
   - the first record banner stat, `Days graded`: the count itself;
   - the first figure of the shadow box `.record-line`: the shadow-day count;
   - the expiry-return chart: one bar per graded day, and the viewBox width, which is computed from the count (see step 4).

   No other text on the page states the count, and no edit may add one that does. See the standing rule on narrative prose below.
4. Rebuild the expiry-return chart (the `figure.chart` block after the table) from the graded table. Geometry is fixed; do not improvise it:
   - One bar per graded day, chronological order, oldest first (the reverse of the table order). Each bar's value and label are exactly that day's Expiry return string from the table.
   - Scale is linear: 0.62 px per percentage point. The 0% line is at y=132. Gridlines at +100% (y=70.0), 0% (y=132.0), and -100% (y=194.0), drawn from x=48 to viewBox width minus 20, with axis labels right-anchored at x=42.
   - Bars are 54 px wide on an 82 px pitch; the first bar starts at x=48. A positive bar (class `bar-pos`) rises from the zero line; a negative bar (class `bar-neg`) hangs below it. Each `rect` carries a `<title>YYYY-MM-DD: value</title>`.
   - Value labels (class `val`) sit centered over the bar: 6 px above a positive bar's top, 14 px below a negative bar's bottom. Date labels (class `axis`, format `DD Oct`) sit centered under each bar at y=224.
   - viewBox width is 82 times the number of graded days, plus 66 (640 when the count is 7). Always compute it from the current count; never carry the previous width forward. viewBox height is always 232.
5. If a past figure was wrong, do not overwrite the row silently. Add a dated correction note below the table and commit it as its own commit explaining the change.
6. Commit with a message naming the date and result, for example: `Day 8 graded: 2026-11-02 WIN, expiry +80.0%, shadow -24.0%`.

## Standing rules

- One row per market weekday. If US equities are closed, no row and no post.
- Losses get the same format and prominence as wins. No row is ever deleted.
- Entry, Peak, and Expiration are always the three price columns, in that order, with exactly those labels.
- The chart is rebuilt from the table at every grading update. Its values must match the Expiry return column exactly, and its bar order is always chronological.
- No language beyond the frozen format: no commentary, no forecasts, and no advice anywhere on the page. The autopsy reasoning stays in the project files, not on the site.
- Narrative prose must never hard-code the sample size. The graded-day count lives only in the five machine-updated elements listed in afternoon step 3. Prose that needs the sample size refers the reader to the record banner instead of stating a number: the 'How to read this' section is worded that way on purpose, including its illustrations drawn from permanent rows, which are phrased without counting them. Do not "fix" that wording by inserting a number; a number in prose is a number that goes stale at the next grading.
- Rules text on the page changes only when MODEL-RULES.md changes at a scheduled review. Next review: Thu 2026-12-17.

## Site files beyond the daily workflow

- `robots.txt`: allows all crawlers and points them at the sitemap. Static. The daily jobs do not maintain it.
- `sitemap.xml`: lists the ledger (`https://0dtepick.com/`), the history page, the archive, and every per-day page. Static. Update the lastmod dates by hand when a listed page changes materially, and add a day page here when one is created.
- `history.html`: the history of the experiment, linked from the ledger footer. Its day-by-day list is updated manually at milestones. It is not one of the five machine-updated elements listed in the afternoon update, and the daily jobs do not maintain it.
- `archive.html`: the index of every graded day, linked from the ledger footer and from `history.html`, listing each day with its result pill and linking to that day's own page. Static between day-page additions.
- `days/day-NN.html`: one page per graded day, carrying the full graded row (the three price columns under their Entry, Peak, and Expiration labels), the shadow result, and the one-line lesson recorded in that day's autopsy. Day pages for future days are added by the daily publishing flow only when instructed; for now they are manual, milestone files like `history.html`, and the daily jobs do not maintain them.
- `og-image.png`: the 1200 by 630 share image referenced by the Open Graph tags on every page. Static. It is a binary file, so it cannot travel the text-only snapshot push route, and the git SSH route is read-only for this repository (the SSH key is a per-repo deploy key whose write access covers DC-Prism only). The rendered file is staged in the local site directory; its upload to the repository is pending a write route, and until it lands the `og:image` URLs point at a file the host does not have yet.
- Traffic counting: every page (the ledger, `history.html`, `archive.html`, and each day page) carries the GoatCounter snippet for site code `0dtepick` just before the closing body tag. It is static markup. The daily jobs do not maintain it, and any new page added to the site must include it.
- `favicon.svg`: the site icon, a dark rounded square with an amber 0, referenced from the head of both pages. Static.
