# Spotify Streaming Analytics: what really drives streams?

**An interactive Excel dashboard, a statistical audit and a machine-learning study of 85,000 tracks, built to answer one question: what actually moves stream counts, and what does not?**

![Excel 2016+](https://img.shields.io/badge/Excel-2016%2B-217346?logo=microsoftexcel&logoColor=white)
![No macros](https://img.shields.io/badge/No%20macros-formulas%20only-1ED760)
![Python](https://img.shields.io/badge/Python-pandas%20%7C%20scikit--learn-3776AB?logo=python&logoColor=white)
![Validated](https://img.shields.io/badge/Validated-40%2F40%20checks-1ED760)
![License](https://img.shields.io/badge/License-MIT-blue)

![Dashboard](images/dashboard.png)

> **Honest data note.** A formal audit (chi-square and KS tests, rounding and clipping checks, feature-independence tests) shows this dataset has the fingerprints of **synthetic data**. This project therefore tests rigorously *what the file contains*, reports "no effect" where the data shows none, and builds a pipeline you can re-run unchanged on real streaming data. See the **Data Audit** page.

---

## What the data says

| # | Finding | Evidence |
|---|---|---|
| 1 | **Popularity is the only engine.** | Tracks above popularity 80 are 3.2% of the catalogue but earn **81.6%** of streams. Hit probability: 0.8% at 66-80, 29.0% at 81+, **40.1% above 85**. |
| 2 | **Sound does not sell.** | A model with *everything except popularity* explains **0.0%** of variance (R2 = -0.001, AUC 0.49). Popularity alone: R2 = 0.634, AUC 0.99. |
| 3 | **Genre, country and label are equals.** | None of 11 segment tests is significant after Holm correction (smallest adjusted p = 0.12). Genre spread: permutation p about 0.085. |
| 4 | **Streaming is winner-take-most.** | Top 1% of tracks = **73.5%** of streams; Gini 0.97; median track 2,000 streams vs mean 214,000. |
| 5 | **Hits, not superstars.** | The top 10 of 62,391 artists hold 1.5% of streams; one track delivers ~70% of a multi-track artist's streams. |
| 6 | **Genres are acoustically identical.** | Genre predicted from audio features at 8.32% accuracy (chance 8.33%). |

Full write-up: [docs/INSIGHTS.md](docs/INSIGHTS.md)

---

## Quick start

1. Download **`dashboard/Spotify_Dashboard.xlsx`**.
2. Open in Excel 2016 or newer and click **Enable Editing**.
3. Use the five green **filter buttons** on the left (Year, Genre, Country, Label, Popularity). Choose *All* to clear.
4. Use the left menu: **Dashboard, Recommendations, Data Table, Analytics, Data Audit**.

No macros, no add-ins. To reproduce the analysis:
```bash
pip install -r requirements.txt
jupyter notebook analysis/spotify_analysis.ipynb
```

---

## The dashboard (designed after the reference)

| Reference element | In this project |
|---|---|
| Logo, nav buttons (Dashboard / Recommendations / Data Table) | Logo, **5 hyperlinked nav buttons** with icons and an active state |
| Artist / Year / Genre slicers | **5 filter cards** (Year, Genre, Country, Label, Popularity tier) as green dropdown buttons |
| 4 rounded KPI cards with icons | Total Streams, Most Streamed Song, Top Artist, Most Popular Genre |
| Best Performing Artists, Most Streamed Songs | Ranked, gradient bar charts (exact under every filter) |
| Streaming Trends by Year | Smooth trend line with selected-year marker |
| Modality pie, Top 5 Genre donut, Number of Songs by Artist | Same panels, live |
| Top 3 Songs by Playlist Appearances | **Top 3 by Popularity** and **Biggest Over-performers** with medal icons (the file has no playlist data) |
| Essential Streaming Insights text | Dynamic insight text that re-writes itself with the filters |
| *(added)* **Map** | **World bubble map** (real country polygons) + country leaderboard |
| *(added)* Popularity panels | Share of tracks vs streams, hit rate, average streams per track |

### Other pages
| Page | What it gives you |
|---|---|
| **Recommendations** | 6 evidence-backed action cards with priority chips |
| **Data Table** | Live **top 100 tracks** for the current filters + an **Artist Spotlight** lookup |
| **Analytics** | 8 charts (drivers, model comparison, permutation importance, concentration curve, hit probability, genre vs uniform, release cohorts) + the segment-test table |
| **Data Audit** | Authenticity tests, evidence and the verdict |

### Gallery
| Recommendations | Data Audit |
|---|---|
| ![Recommendations](images/recommendations.png) | ![Data Audit](images/data_audit.png) |

![Analytics](images/analytics.png)

![Data Table](images/data_table.png)

---

## Analytics performed
Data audit and authenticity testing; concentration (Lorenz curve, Gini, top-p% shares); popularity-to-streams modelling; **5-fold cross-validated gradient boosting** with and without popularity (R2 and ROC AUC); permutation importance; segment tests with **Holm correction** and **permutation tests**; artist-level analytics; a breakout (over-performer) score; and actionable recommendations.

## Engineering and validation
- About **600,000 formulas**, no macros, Excel 2016 compatible. Artist rankings are **exact for any filter combination** (the Data sheet is sorted by artist and carries running per-artist totals).
- **Validated against independent pandas calculations in 4 filter scenarios** (no filters; Year 2019 + Pop; Japan + EMI + High popularity; Year 2022 + Jazz + Canada): totals, track and hit counts, top song, top-10 artists, top-5 genres, country and year contexts, modality, popularity bands, songs-by-artist, top-3 popularity and breakout. **40 of 40 checks pass.**
- During validation a 100-track candidate window mis-ranked artists because 458 tracks tie at the 20M stream cap; this was replaced by the exact method and re-validated.

Methods: [docs/METHODOLOGY.md](docs/METHODOLOGY.md) | Columns: [docs/DATA_DICTIONARY.md](docs/DATA_DICTIONARY.md)

## Limitations
Synthetic-looking data; no time series of streams (the trend is by *release year*); `country` has no documented meaning; `artist_name` is not a unique ID; results are associations, not causes.

## Repository structure
```
.
├── README.md  LICENSE  NOTICE.md  requirements.txt
├── dashboard/Spotify_Dashboard.xlsx
├── data/spotify_tracks.csv.gz
├── analysis/spotify_analysis.ipynb      (executed, with outputs)
├── docs/INSIGHTS.md  DATA_DICTIONARY.md  METHODOLOGY.md
└── images/
```

## Author
Ogunleti Gideon | Data Analyst | [GitHub](https://github.com/Etebo10)

## License
MIT (see [LICENSE](LICENSE)). Icon, map and font attributions, and the trademark disclaimer, are in [NOTICE.md](NOTICE.md).
