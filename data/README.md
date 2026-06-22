# Vendored datasets

These datasets are vendored here so the code chunks in the cookbook are
reproducible. They were originally read from instructor URLs or from local
course folders. Raw numeric data of this kind (measurements, textbook problem
sets) is generally uncopyrightable fact and these sets are already widely
mirrored; each is cited below in good faith with its source and retrieval date.

All files retrieved / collected **2026-06-11**.

Task pages read these via a relative path (`../../data/<file>`); no analysis
logic was changed — only the path string.

*Organized using Claude Code*

## Experiments — Stat 6410, Design and Analysis of Experiments {#experiments}

Source: Dean, Voss & Draguljic, *Design and Analysis of Experiments* (2nd ed.),
companion data site.
Original URL pattern: `http://deanvossdraguljic.ietsandbox.net/DeanVossDraguljic/R-data/<file>`
Retrieved 2026-06-11 (HTTP 200).

| File | Original URL |
|---|---|
| `trout.txt` | …/R-data/trout.txt |
| `reaction.time.txt` | …/R-data/reaction.time.txt |
| `bicycle.txt` | …/R-data/bicycle.txt |
| `water.boiling.txt` | …/R-data/water.boiling.txt |
| `paper.towel.strength.txt` | …/R-data/paper.towel.strength.txt |
| `weld.strength.txt` | …/R-data/weld.strength.txt |

## Statistical Computing — Stat 6730, Introduction to Statistical Computing {#statistical-computing}

Source: OSU Stat 6730 course materials (V. Q. Vu).
Original URL pattern: `https://www.stat.osu.edu/~vqv/6730/data/<file>`
Retrieved 2026-06-11 (HTTP 200). The OSU URLs currently resolve via a 301
redirect to a personal page (`asc.ohio-state.edu/vu.104/...`), which is fragile
— hence vendoring.

| File | Original URL |
|---|---|
| `fev.RData` | …/6730/data/fev.RData |
| `lab_05.R` | …/6730/data/lab_05.R |
| `lab_05.RData` | …/6730/data/lab_05.RData |
| `fish.txt` | …/6730/data/fish.txt |
| `ozonerats.csv` | …/6730/data/ozonerats.csv (permutation test) |
| `electemp.csv` | …/6730/data/electemp.csv (cross-validation) |

## Time Series — Stat 6550, Statistical Analysis of Time Series {#time-series}

Source: OSU Stat 6550 course materials. Collected 2026-06-11.

| File | Description |
|---|---|
| `US_oil_production.txt` | Monthly U.S. oil production (360 obs from 1980), used for the moving-average trend, seasonal decomposition, and ARMA-fitting tasks. (The wind-speed and Columbus-temperature series for the other Time Series tasks are entered inline in those pages.) |

## Multivariate — Stat 6560, Applied Multivariate Analysis {#multivariate}

Source: Johnson & Wichern, *Applied Multivariate Statistical Analysis* (6th ed.),
textbook data sets (the `Data_JW6` collection).
Collected from local course folder 2026-06-11.

| File | Description |
|---|---|
| `T1-8.DAT` | Table 1.8 (mineral content of bones) |
| `T4-6.DAT` | Table 4.6 (psychological profile data) |

## Regression — Stat 6450, Applied Regression Analysis {#regression}

Source: Kutner, Nachtsheim, Neter & Li, *Applied Linear Statistical Models*
(ALSM), textbook problem data sets. File names follow the textbook's
`CH<chapter>PR<problem>` convention.
Collected 2026-06-11 (local course folders / public ALSM mirror).

| File | Used for |
|---|---|
| `gpa.txt` | GPA simple linear regression |
| `CH03PR03.txt` | residual / assumption checking |
| `CH03PR15.txt` | concentration data |
| `CH03PR17.txt` | sales data |
| `CH06PR15.txt` | patient satisfaction (multiple regression) |
| `CH01PR19-1.txt` | GPA vs. ACT |
| `CH08PR16.txt` | indicator variable (paired with `CH01PR19-1`) |
| `CH06PR18-1.txt` | commercial property rental rates |
| `CH09PR10-1.txt` | job / model-selection data |
| `CH09PR22-1.txt` | job proficiency (model selection) |
| `CH14PR07.txt` | logistic regression (dues) |
| `CH14PR14.txt` | logistic regression (vaccination) |
