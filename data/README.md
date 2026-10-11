# Data

## What is not here

The dataset itself (`EVChargingStationUsage.csv`, about 85 MB, 259,415 rows) is **not committed**. It is third-party data, it is close to GitHub's file-size limits, and the report reads it from a local folder.

Download it from either source:

- **Kaggle:** [EV Charging Station Usage of California City](https://www.kaggle.com/datasets/venkatsairo4899/ev-charging-station-usage-of-california-city) (the copy used for this analysis)
- **City of Palo Alto open data portal:** *Electric Vehicle Charging Station Usage (July 2011 – Dec 2020)*. Note that the portal's table export is capped at 10,000 rows, so use the full-file download.

Save it as `EVChargingStationUsage.csv` in any folder, then point the `p_DataFolder` parameter in Power Query at that folder.

## What the analysis used

- Coverage: 29 July 2011 – 31 December 2020
- 259,415 raw rows → 259,384 sessions after removing 17 duplicates and 14 non-USD sessions
- 46 ports across 9 sites

The cleaning steps are in [`../docs/02-data-model.md`](../docs/02-data-model.md).

## Licence

Listed on Kaggle as *U.S. Government Works*. The original publisher is the City of Palo Alto.
