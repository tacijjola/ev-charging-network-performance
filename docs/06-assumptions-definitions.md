# Assumptions, limitations and definitions

## Assumptions and limitations

- **Port-days.** Each port is in service from its first session to the end of the data. Outages and decommissioning aren't recorded, so per-port figures are understated for any port that was offline.
- **Utilisation is plugged-in time**, not charging time. It includes idle time (19.6% network-wide), so high utilisation can mean parked cars as well as charging demand.
- **Sherman** opened in December 2020 with 39 sessions. It is shown but not compared. Site Detail flags any site with fewer than 500 sessions.
- **Sites** are grouped by station name. Bryant and Cambridge each span two addresses.
- **Revenue** is in USD.
- **User analysis** covers 97% of sessions; 7,677 sessions have no user ID.
- **User bands and deciles** use lifetime session counts and don't respond to slicers. A user who became a regular later is labelled Frequent in every year, which understates shifts in the user mix.
- **Comparisons.** Fees: 2015–16 against 2018–19. 2017 is excluded as a mixed year (fees from 1 August) and 2020 because of COVID-19. A change within ±5% is treated as no meaningful change. Idle time: 2016 against 2019. The results show timing, not cause.
- **Time.** Sessions are counted on their start date and start hour; overnight sessions aren't split across days. 2011 covers July–December only, so growth is measured from 2012.
- **Chart titles** are calculated and update with the filters. Interpretive notes (COVID-19, fee start date) are fixed text.
- **Layout.** Built for desktop; no mobile layout.

## Definitions

| Term | Definition |
|---|---|
| Port | One charging connector. One row in `Dim_Station` |
| Port-days | Days each port was in service within the selected period, summed across ports |
| kWh per port per day | Energy delivered ÷ port-days |
| Sessions per port per day | Sessions ÷ port-days |
| Utilisation % | Plugged-in minutes ÷ (port-days × 1,440) |
| Idle time % | Plugged-in minutes not spent charging ÷ plugged-in minutes |
| Avg sessions per day | Sessions ÷ days in the selected period |
| Fault rate % | Sessions ending in a fault ÷ sessions |
| Paid session share % | Sessions with a fee above zero ÷ sessions |
| Revenue per kWh | Fees ÷ energy delivered |
| Active users | Distinct non-blank user IDs |
| Top 1% session share | Sessions by the most active 1% of users ÷ sessions with a user ID |
| Network measures | The same measure with the site filter removed, used as the Site Detail benchmark |
| User bands (lifetime) | One-off = 1 session · Occasional = 2–10 · Frequent = 11+ |
| Duration bands | <1h, 1–2h, 2–4h, 4–8h, 8h+ |
| Hour bands | Night 00–05 · Morning 06–11 · Midday 12–17 · Evening 18–23 |
| Idle-peak hours | 05:00–23:00; the evening peak uses hours with at least 1% of sessions |
