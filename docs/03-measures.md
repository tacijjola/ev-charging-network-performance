# Measures

The measures that carry the analysis, and the reasoning behind each. This page explains the design; the semantic model in [`../report/`](../report) is the source of truth for exact definitions. Measures live in dedicated measure tables grouped by purpose (core volume, sessions, operations, reliability, time, users, rankings, data quality, dynamic titles, benchmarks).

---

## The denominator that changed the answer

```dax
Port Days =
SUMX (
    Dim_Station,
    VAR FirstDay = Dim_Station[FirstSessionDate]
    RETURN
        COUNTROWS ( FILTER ( VALUES ( Dim_Date[Date] ), Dim_Date[Date] >= FirstDay ) )
)
```

`Dim_Station` has one row per port, so this sums, port by port, the days each port has been in service **within the current filter**. `VALUES(Dim_Date[Date])` takes the slicer and axis context, so filtered energy is always divided by filtered days.

The first version divided by every port × every day since 2011. A site opened in 2016 was judged on five years it didn't exist, and the per-port ranking was wrong. See the [validation log](04-validation-log.md).

Everything per port is built on it:

| Measure | Definition |
|---|---|
| `Energy per Port per Day (kWh)` | Energy delivered ÷ `Port Days` |
| `Sessions per Port per Day` | Sessions ÷ `Port Days` |
| `Utilisation %` | Plugged-in minutes ÷ (`Port Days` × 1,440) |
| `Idle Time %` | Plugged-in minutes not spent charging ÷ plugged-in minutes |

```dax
Ports = COUNTROWS ( Dim_Station )
```

Never `SUM(Dim_Station[PortCount])`: `PortCount` is the site total repeated on every port row.

---

## Benchmarks: every site judged against the whole

```dax
Network kWh per Port per Day =
CALCULATE ( [Energy per Port per Day (kWh)], REMOVEFILTERS ( Dim_Station ) )

Network Utilisation % =
CALCULATE ( [Utilisation %], REMOVEFILTERS ( Dim_Station ) )
```

Removing the station filter but keeping the date filter gives the reference label under every Site Detail card and the network line on the trend charts. The same pattern covers idle time and sessions per port per day.

---

## Shares that sum to 100%

```dax
Session Share by Hour =
DIVIDE ( [Total Sessions], CALCULATE ( [Total Sessions], REMOVEFILTERS ( Dim_Time ) ) )

Band Share =
DIVIDE (
    [Total Sessions],
    CALCULATE (
        [Total Sessions],
        REMOVEFILTERS ( Fact_Sessions[DurationBand], Fact_Sessions[DurationBandSort] )
    )
)
```

`DurationBand` is sorted by a helper column. Power BI filters **both** columns behind each bar, so removing the filter from `DurationBand` alone left the sort column filtering the denominator, and every band showed 100%. Removing both fixes it.

```dax
Avg Sessions per Day =
DIVIDE ( [Total Sessions], DISTINCTCOUNT ( Dim_Date[Date] ) )
```

Used for the weekday-versus-weekend comparison. Five weekdays against two weekend days makes totals misleading; per day, weekends run at about 78% of weekday volume.

---

## Reliability

```dax
Fault Rate % =
IF (
    [Total Sessions] > 0,
    DIVIDE ( [Fault Sessions] + 0, [Total Sessions] )
)
```

`+ 0` shows 0.00% for a port that had sessions but no faults. The `IF` keeps the measure blank where there were no sessions, so tables don't fill with empty port-year rows.

---

## Users

| Measure | Definition |
|---|---|
| `Active Users` | Distinct non-blank user IDs (`DISTINCTCOUNTNOBLANK`) |
| `Avg Sessions per User` | Sessions with a user ID ÷ active users |
| `Top 1% Session Share` | Sessions by the most active 1% of users ÷ sessions with a user ID. Dynamic: responds to slicers |
| `Paid Session Share %` | Sessions with a fee above zero ÷ sessions. Returns 0% (not blank) before fees, so the step change shows |
| `Avg Idle Minutes` | (Plugged-in minutes − charging minutes) ÷ sessions |

The Pareto chart uses calculated columns on `Dim_User`: lifetime `User Sessions`, a unique rank and a decile.

```dax
User Rank =
ROWNUMBER (
    ALL ( Dim_User[User ID], Dim_User[User Sessions] ),
    ORDERBY ( Dim_User[User Sessions], DESC, Dim_User[User ID], ASC )
)
```

`RANKX` gave tied users the same rank, so thousands of single-session users landed in one decile and others were left empty. `ROWNUMBER` with a user-ID tie-break gives ten equal groups. Passing only the two columns it needs also avoids a circular dependency with the decile column.

---

## Dynamic titles and insight lines

Every finding title is a measure applied through the visual's title **fx** setting. The pattern:

1. Compute the fact (the top site, its value, the network value).
2. Guard the claim: state it only when the data supports it, otherwise fall back to a neutral description.
3. Handle edge cases: a single selected site, or a selection too thin to rank.

Example, the scatter title:

```dax
Title — Scatter =
VAR SiteTbl =
    ADDCOLUMNS (
        FILTER ( ALLSELECTED ( Dim_Station[SiteName] ), [Total Sessions] >= 500 ),
        "P", [Ports],
        "K", [Energy per Port per Day (kWh)]
    )
VAR TopSite  = MAXX ( TOPN ( 1, SiteTbl, [K], DESC, Dim_Station[SiteName], ASC ), Dim_Station[SiteName] )
VAR BigSite  = MAXX ( TOPN ( 1, SiteTbl, [P], DESC, [K], ASC ), Dim_Station[SiteName] )
VAR TopPorts = MAXX ( FILTER ( SiteTbl, Dim_Station[SiteName] = TopSite ), [P] )
VAR BigPorts = MAXX ( FILTER ( SiteTbl, Dim_Station[SiteName] = BigSite ), [P] )
RETURN
    IF (
        TopSite = BigSite,
        "Here the largest site is also the busiest: " & TopSite & " (" & TopPorts & " ports)",
        "Bigger sites are not busier — " & TopSite & "'s " & TopPorts & " ports out-deliver "
            & BigSite & "'s " & BigPorts
    )
```

Sites with fewer than 500 sessions in the current selection are excluded from rankings, which keeps a site that opened last month from topping a per-port chart. Interpretation that DAX can't know, such as COVID-19 or the fee start date, stays as fixed subtitle text and is reviewed by hand at each refresh.

Two conventions throughout: variable names say what they hold (`IdleBefore`, `TopSite`), which also avoids DAX reserved words; and ties are always broken alphabetically, so `TOPN(1, …)` returns exactly one row.
