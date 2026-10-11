# Report

The Power BI report, saved as a **Power BI Project** (`.pbip`) rather than a single `.pbix`.

| Path | What it is |
|---|---|
| `EV_Charging_Report.pbip` | Open this in Power BI Desktop |
| `EV_Charging_Report.Report/` | Pages, visuals and layout |
| `EV_Charging_Report.SemanticModel/` | Tables, relationships, Power Query and every DAX measure, as text |

The semantic model's `definition/` folder is where the measures can be read directly on GitHub. Cached data (`.pbi/cache.abf`) and local settings are excluded by `.gitignore`, so the repository holds the model, not the data.

To open it, download the dataset (see [`../data/README.md`](../data/README.md)), open the `.pbip`, set `p_DataFolder` in Power Query and refresh.
