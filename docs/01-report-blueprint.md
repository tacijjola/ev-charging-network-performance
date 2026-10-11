# Report blueprint
Palo Alto public charging network · 29 Jul 2011 – 31 Dec 2020 · 259,384 sessions · 46 ports · 9 sites
Status: all 7 pages built and published. This is the design the report was built from, updated after validation.

---

## The argument the report makes

> **Demand is concentrated in a few small, busy sites and a small core of users. Pricing reduced idle time but also reduced demand, so capacity decisions should follow where usage already is.**

Three supporting findings:
1. **Bigger sites are not busier.** Webster's 3 ports deliver 62.9 kWh per port per day, 1.9× the network's 33.9. MPL's 6 ports deliver 17.8, with its weakest ports far below the rest.
2. **Fees changed behaviour in both directions.** Paid charging from 1 Aug 2017 coincided with 26% fewer sessions per port (2015–16 vs 2018–19) and idle time falling from 44 to 18 minutes per session (2016 vs 2019).
3. **Demand is concentrated.** The top 1% of users generate 31% of sessions; the most active 10% generate about three-quarters.

---

## Page 1 — Executive Overview
**Question: how big is this network, and is it growing?**

| Element | Visual | Field / measure |
|---|---|---|
| Header | Text box + dynamic subtitle | Report title · Palo Alto · years · sessions |
| KPI row (5) | Card | Energy delivered (MWh) · Charging sessions · Unique drivers · kWh per port per day · Time plugged in but idle |
| Insight line | Card | `Insight Line` (dynamic: top site per port vs network, top site by energy) |
| Growth | Line + column combo | Energy (MWh) by Year, sessions as line |
| Per-port ranking | Column, sorted desc | Energy per Port per Day by SiteName, top site highlighted |
| Scale check | Bar, sorted desc | Total Energy (MWh) by SiteName |
| Filters | Slicers | Year range, SiteName, Clear filters |

**Conclusion:** demand grew about 10× from 2012 to 2019, then halved in 2020 (COVID-19). 2011 covers Jul–Dec only, so growth is measured from 2012. Bryant delivers the most energy (387.2 MWh, 17.5%), but Webster works hardest per port.

---

## Page 2 — Demand Patterns
**Question: when is the network busy, and what does that mean for capacity?**

| Element | Visual | Field / measure |
|---|---|---|
| Peak map | Matrix heat map | Weekday × HourOfDay, Total Sessions |
| Evening idle | Column + line | Sessions and Idle Time % by HourOfDay |
| Daily shape | Area | **Avg Sessions per Day** by HourOfDay, split by DayType |
| Dwell distribution | Column | Share of sessions by DurationBand |
| Seasonality | Line | Sessions by month, 2018 / 2019 / 2020 |
| Filters | Slicers | Year, SiteName, HourBand |

**Conclusion:** demand peaks 11:00–13:00 on weekdays, with a second peak at 18:00. Weekends run at 78% of weekday volume per day. 12% of sessions run over 4 hours (3% over 8). Evening charging is mostly parking: idle share peaks around 22:00.

---

## Page 3 — Site & Station Performance
**Question: which sites earn their hardware?**

| Element | Visual | Field / measure |
|---|---|---|
| Insight line | Card | `Insight Line — Sites` (guarded expansion claim) |
| KPI row (4) | Card | Sites · Ports · kWh per port per day · Port-time utilisation |
| Map | Azure map | Bubble size = total energy |
| Efficiency scatter | Scatter | X = Port Days, Y = kWh per port per day, size = Total Energy (MWh), network constant line at 33.9 |
| Utilisation | Bar | Utilisation % by SiteName, top site highlighted, dynamic title |
| Footnote | Text | Sherman: too new to compare |

**Conclusion:** Webster is the strongest case for expansion: 62.9 kWh per port per day, 74.5% utilisation, all 3 ports at or above 64%. Large sites (Bryant, MPL) sit below the network line.

---

## Page 4 — Site Detail (drill-through)
**Question: how does one site and each of its ports perform against the network?**

Drill-through on `Dim_Station[SiteName]` from Pages 1 and 3.

| Element | Visual | Field / measure |
|---|---|---|
| Header | Card | `Site Detail Title` (ports, in-service date, sample-size warning under 500 sessions) |
| KPI row (4) | Card + reference labels | Site value with Network benchmark |
| Trend | Line | Site vs network kWh per port per day by Year |
| Hourly profile | Line | Session Share by Hour vs Network Session Share by Hour |
| Per-port table | Table + data bars | Station, first session, sessions, MWh, kWh per port per day, utilisation, idle, fault rate |
| Dwell | Clustered column | Band Share vs Network Band Share |

**What it reveals:** MPL's low ranking comes from two weak ports; Webster's pressure is site-wide; Cambridge's per-port output fell after expanding from 2 to 5 ports in 2018.

---

## Page 5 — Users & Revenue
**Question: who uses the network, and what happened when charging stopped being free?**

| Element | Visual | Field / measure |
|---|---|---|
| KPI row (5) | Card | Active Users · Avg Sessions per User · Top 1% Session Share · Total Revenue · Revenue per kWh |
| Insight line | Card | `Insight Line — Users` |
| Fees vs demand | Column + line (secondary axis) | Sessions per Port per Day, Paid Session Share % (step), constant line "Fees from Aug 2017" |
| Behaviour | Line | Avg Idle Minutes, Avg Session Minutes by Year |
| Concentration | Pareto (column + line) | Share of sessions and cumulative share by User Decile |
| User mix | 100% stacked column | Sessions by UserBand by Year |

**Conclusion:** fees coincided with 26% fewer sessions per port and much shorter idle time. The most active users carry the network; frequent users' share of sessions fell from 86% (2016) to 75% (2019). Wording stays "coincided with": the data shows timing, not cause.

---

## Page 6 — Data & Model
Source text (Kaggle dataset, licence as listed) · freshness and volume cards · star schema screenshot · raw → analysed reconciliation · data quality flags.

## Page 7 — Assumptions & Validation
Assumptions & limitations · measure definitions · validation & corrections log.

---

## Consistency rules
- **Titles state a finding.** Facts in DAX; interpretation (COVID, causes) stays typed and is reviewed by a human.
- **Guard every claim.** A dynamic title only states a finding when the data supports it; otherwise it falls back to a neutral description.
- **Units in labels, symbols in values.** "Energy delivered (MWh)" → 2,216. % and $ stay with the number.
- **One decimal** for kWh and percentages everywhere.
- **Colours:** navy `#1F3864` for the metric in focus, `#7FA7CE` for context/network. Gold only as a second series on Pages 1–2.
- **Terminology:** "ports" throughout (port = one charging connector).
- Same header, slicer row, card style and Clear filters button on every analytical page.
- DAX variable names are descriptive (`IdleBefore`, `TopSite`) to avoid reserved words.

---

## Key numbers

| Figure | Value |
|---|---|
| Sessions | 259,415 raw → 259,384 analysed |
| Energy | 2,216 MWh |
| Ports / sites | 46 / 9 |
| Period | 29 Jul 2011 – 31 Dec 2020 |
| Network kWh per port per day | 33.9 |
| Network port-time utilisation | 41.1% |
| Network idle share | 19.6% |
| Best site per port | Webster, 62.9 kWh, 74.5% utilisation, 3 ports |
| Weakest established site | MPL, 17.8 kWh, 6 ports (weakest ports at 3.9 and 6.0) |
| Largest site by energy | Bryant, 387.2 MWh (17.5%) |
| Growth | ~10× from 2012 to 2019; halved in 2020 |
| Peak | 11:00–13:00 weekdays; second peak 18:00 |
| Fees | From 1 Aug 2017 (all sites except Sherman); sessions per port −26% |
| Idle time | 44 → 18 minutes per session (2016 → 2019) |
| Concentration | Top 1% = 31% of sessions; top 10% ≈ 75% |
| Users | 21,436 active users · 11.7 sessions per user |
| Revenue | $284.7K · $0.13 per kWh |

---

## Findings register

Moved to its own file: [05-findings-register.md](05-findings-register.md).

---

## Refresh checklist (if the data is updated)
1. Check Page 6 freshness cards: last session date moved, row counts plausible.
2. Check data quality flags for jumps.
3. Read every dynamic title and insight line; confirm guards still hold.
4. Review typed interpretation (COVID note, fee date) and the findings register.
