# Insights and Recommendations

Data: 85,000 tracks released 2015-2025, 18.22 billion streams. All numbers match the workbook, the executed notebook and an independent re-computation.

> **Read this first.** The file shows the fingerprints of synthetic data (see [METHODOLOGY](METHODOLOGY.md) and the *Data Audit* page). Findings describe **this file**. The method, dashboard and tests transfer unchanged to real streaming data.

## 1. Streaming is winner-take-most
| Measure | Value |
|---|---:|
| Top 0.1% of tracks (85) | 9.3% of streams |
| Top 1% of tracks (850) | **73.5%** of streams |
| Top 5% of tracks | 94.7% of streams |
| Top 10% of tracks | 97.5% of streams |
| Gini coefficient | 0.969 |
| Median track | 2,000 streams |
| Mean track | 214,000 streams |
| Tracks at the 1,000-stream floor | 38.4% |

**Implication:** averages are meaningless; judge portfolios by hit rate and top-decile share.

## 2. Popularity is the only engine
| Popularity band | % of tracks | % of streams | Mean streams | Hit rate (top 1%) |
|---|---:|---:|---:|---:|
| 0-20 | 1.3% | 0.0% | 1,293 | 0.0% |
| 21-35 | 17.4% | 0.2% | 1,906 | 0.0% |
| 36-50 | 42.3% | 0.9% | 4,499 | 0.0% |
| 51-65 | 27.1% | 3.5% | 27,452 | 0.0% |
| 66-80 | 8.6% | 13.9% | 347,003 | 0.8% |
| 81-100 | 3.2% | **81.6%** | 5,431,329 | **29.0%** |

- Hit probability above popularity 85 is **40.1%**; between 81 and 85 it is 9.0%; below 66 it is 0.0%.
- Rank correlation of popularity with streams: **0.64**. A log-linear fit (R2 = 0.55) says each +10 popularity points multiplies streams by about 2.6x, although the real relationship is steeper above 80.
- **Tier view:** the 11.8% of tracks labelled High popularity (66+) earn 95.5% of streams.

## 3. Nothing else matters (tested, not assumed)
| Model (5-fold CV, log streams) | R2 | ROC AUC for "is a hit" |
|---|---:|---:|
| Popularity only | **0.634** | **0.992** |
| Everything except popularity | -0.001 | 0.49 |
| Everything | 0.633 | 0.989 |

- Permutation importance: popularity 1.296; every other feature, including genre, country and label combined, about 0.001.
- Rank correlations with streams for danceability, energy, loudness, tempo, instrumentalness, duration, explicit and release year are all below 0.006 in absolute value (largest: loudness, 0.0053).
- **Segment tests** (Kruskal-Wallis on log streams, Holm-corrected, 11 variables): **none significant** (smallest adjusted p = 0.12, genre). Genre spread of mean streams: permutation p about 0.085. Year-to-year swings in total streams: permutation p about 0.29 (both Monte Carlo estimates, +/- 0.02).
- Market shares are flat: every country holds 8.8-11.1% of streams, every label 11.6-13.5%, every genre 6.9-10.5% (the apparent leader, Hip-Hop at 10.5%, is created by a handful of capped mega-tracks).

## 4. Artists
- 62,391 artist names; the top 10 hold only **1.5%** of streams. The top 1% of artists hold 64.3%, which is just the track-level concentration again.
- For artists with 3+ tracks, the best track delivers **70%** of their streams on average.
- `artist_name` is not a unique ID (one name covers 44 tracks): artist-level results are indicative.

## 5. Breakout over-performers
Breakout score = streams divided by the streams expected at that popularity. The 99th percentile is **27x**; the maximum is **618x** (a popularity-68 track at the 20M cap). These are the tracks popularity cannot explain: the learning set for playlisting, virality and collaboration effects.

## Recommendations
1. **Back hits early.** Focus budget on tracks with popularity momentum (65+); hit probability rises from 0.8% (66-80) to 29% (81+).
2. **Do not pay for audio or genre optimisation.** Models without popularity explain 0% of variance.
3. **Treat genre, market and label as equals.** No segment effect survives multiple-testing correction.
4. **Plan for a long tail.** Use hit rate and top-decile share as KPIs.
5. **Study breakout tracks** for non-popularity drivers.
6. **Validate on production data** before acting.

## Limitations
Synthetic-looking data; no time series of streams (the "trend" is by release year); `country` has no documented meaning; artist names are not unique; effects are descriptive, not causal.
