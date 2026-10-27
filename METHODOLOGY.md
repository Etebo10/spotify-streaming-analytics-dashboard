# Methodology

## 1. Is the data real? (authenticity audit)
Before modelling, the file was tested for signs of simulation.

| Fingerprint | Result |
|---|---|
| Genre, country, label, key, weekday counts vs uniform (chi-square) | p = 0.14 to 0.93: indistinguishable from uniform random draws |
| Release dates | Uniform over the calendar (month counts follow days per month, p = 0.53) |
| Danceability, energy, instrumentalness, tempo, loudness, duration | Uniform (KS D below 0.01) |
| Mode | Exactly 50.0% Major / Minor |
| Stream counts | 100% multiples of 1,000; 38.4% at the 1,000 floor; 458 tracks exactly at 20,000,000 (clipped) |
| Loudness | Mean -28 dB (real catalogues cluster near -8 dB) |
| Audio-feature correlations | Maximum 0.007; genre recovered from audio at 8.32% accuracy (chance 8.33%) |
| Popularity | Bell-shaped (mean 48.2, sd 14.8), clipped at 0 and 100; the only variable with real structure |
| Names | 18.9% repeated track names; one artist name covers 44 tracks |

**Conclusion:** the file behaves like a simulation. The analysis is therefore framed as a rigorous test of *what the file contains*.

## 2. Statistical methods
- **Concentration:** Lorenz curve, Gini, top-p% shares.
- **Popularity model:** log-linear regression, Spearman correlation, banded hit rates.
- **Machine learning:** gradient boosting (`HistGradientBoosting`), 5-fold cross-validated R2 on `log(1+streams)` and ROC AUC for the hit label. Three feature sets: popularity only, everything except popularity, everything. `upbeat_score` and `log_stream_count` were excluded as redundant.
- **Importance:** permutation importance on a held-out set (categorical dummies grouped back to genre, country, label).
- **Segment effects:** Kruskal-Wallis on log streams with **Holm correction** across 11 variables; epsilon-squared effect sizes; permutation tests (genre spread of mean streams, release-year CV) to show differences are within chance.
- **Breakout score:** streams / expected streams, expected = exp of the 5-point smoothed mean of log streams at each popularity value.

## 3. Dashboard engineering (Excel 2016)
- **Filters** are five dropdown cells (Year, Genre, Country, Label, Popularity tier). A `Match` column on the Data sheet flags each row; every aggregate is a single-criterion `SUMIFS` / `COUNTIFS` over it.
- **Rankings** (top songs, artists, genres, countries) use `LARGE` over tie-broken helper keys, so results are deterministic even when many tracks tie.
- **Charts that must show the full comparison** (trend, genre donut, map) ignore their own filter via wildcard criteria.
- **Artist rankings** (Best Performing Artists and Number of Songs) are exact for every filter combination: the Data sheet is sorted by artist and carries running per-artist totals, so no candidate windows are needed.
- **Visuals:** the panels, KPI cards, filter cards, logo and the world map (Natural Earth polygons) are pre-rendered high-resolution graphics; live values sit on top as text boxes linked to cells and as native charts. The bubble map is a native bubble chart aligned to the map image.
- **No macros, no add-ins.** About 600,000 formulas; cached values are embedded so the file displays even in Protected View.

## 4. Validation
Every dashboard measure was re-computed independently in pandas for several filter scenarios (all filters off; Year 2019 + Pop; Japan + EMI + High popularity; Year 2022 + Jazz + Canada): totals, track counts, hit counts, top song, top-10 artists, top-5 genres, country and year contexts, modality, popularity bands, songs-by-artist, top-3 popularity and breakout. A first version of the artist ranking used a 100-track window and mis-ranked artists because 458 tracks tie at the 20M cap; the window was widened to 600 and re-validated. Results are in the repository README.

## 5. Limits
Synthetic-looking data, no time series of streams, undocumented `country`, non-unique artist names, associations not causes.
