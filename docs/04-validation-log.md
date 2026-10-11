# Validation log

Checks performed and corrections made, roughly in the order they happened. The corrections that changed what the report says come first.

## Corrections that changed the findings

**In-service denominator.** The first `Port Days` counted every port as live from July 2011. Late-opening sites looked idle because they were judged on years before they existed; Sherman showed 0.01 kWh per port per day. Counting each port from its first session fixed it. Webster, not Hamilton, emerged as the top site per port, and the blueprint was rewritten.

**Port grain checked before a "fix" went in.** An intermediate version multiplied port-days by `PortCount`, which made per-port figures fall by exactly each site's port count. A table of `COUNTROWS(Dim_Station)` against `MAX(Dim_Station[PortCount])` per site showed one row per port (46 rows = 46 ports), so the original measure was right and the multiplier was reverted.

**Hypothesis tested, not assumed.** The plan expected that fees did not reduce usage. Sessions per port fell 26% from 2015–16 to 2018–19. The report states that finding, worded as "coincided with".

**Missing user IDs.** They were stored as empty text, not null. Empty text counts as a value, so 7,677 anonymous sessions behaved as one very active user, inflating the top decile. They were converted to null in Power Query, and unique-user counts use `DISTINCTCOUNTNOBLANK`.

## Measure and visual checks

- **Share measures sum to 100%.** Hourly shares and duration-band shares were checked in a temporary table for both the site and the network. A sort-by-column filter that forced every duration band to 100% was fixed by removing the filter from both columns.
- **Network measures really are network-wide.** Each benchmark was checked against the full-network card while drilled into a single site. One that returned the site's own value was missing its `REMOVEFILTERS`.
- **Formats checked against meaning.** A sessions-per-port measure carrying a percentage format displayed 4.0 as 396.8%. Every measure's format is set on the measure, not the visual.
- **User deciles.** Tied session counts produced uneven deciles. A unique rank (tie-break on user ID) gives ten groups of 2,144 users each.
- **Weekend comparison normalised per day.** A chart of total sessions put weekends at about a third of weekdays; per day they run at about 78%. The chart and its title now use the same per-day measure.
- **Night hours excluded from the idle peak.** In single-year views, hours with a handful of sessions produced idle spikes at 2–3 am. The idle-peak title only considers evening hours with at least 1% of sessions, and the chart starts at 05:00.
- **Titles that contradicted filtered numbers.** Typed titles ("12% of sessions run over 4 hours") were wrong as soon as a year was selected. Finding titles were rebuilt as DAX measures with guards.
- **Trend charts protected from the year slicer.** On Users & Revenue, the before/after charts ignore the year slicer (Edit interactions → None), because a single year cannot show a before and after.

## Data checks

- **Reconciliation:** 259,415 raw rows − 17 duplicates − 14 non-USD = 259,384 sessions analysed.
- **Fee start:** the first paid session is 1 August 2017 at every site except Sherman, which has none.
- **Charging longer than plugged-in:** 43 sessions. Kept and flagged; negligible effect.
- **Sessions over 24 hours:** 22 (longest 114 hours). Kept; they slightly inflate idle time.
- **Excel-serial dates:** 80 end dates repaired in Power Query.
- **Date table range:** `Dim_Date` does not extend past the last session, so no port is credited with idle days after the data ends.
