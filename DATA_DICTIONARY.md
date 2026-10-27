# Data Dictionary

## `data/spotify_tracks.csv.gz` (85,000 rows x 33 columns)
One row per track. Original columns are untouched.

| Column | Type | Description |
|---|---|---|
| `track_id` | text | Unique track ID (no duplicates) |
| `track_name`, `artist_name`, `album_name` | text | Names (not unique: 68,952 distinct track names, 62,391 distinct artists) |
| `release_date` | date | 2015-01-01 to 2025-12-31 |
| `genre` | text | 12 genres: Classical, Country, EDM, Folk, Hip-Hop, Indie, Jazz, Metal, Pop, R&B, Reggaeton, Rock |
| `duration_ms`, `duration_minutes` | number | Track length (1.5 to 7 minutes) |
| `popularity` | integer | 0-100 popularity score |
| `danceability`, `energy`, `instrumentalness` | decimal | 0-1 audio features |
| `key`, `key_name` | integer / text | Musical key (0-11 / C, C#, ...) |
| `loudness` | decimal | dB, range -55 to -1 (mean -28) |
| `mode`, `mode_name` | 0/1, text | Minor / Major |
| `tempo` | decimal | BPM, 60 to 200 |
| `stream_count` | integer | Total streams for the track, all multiples of 1,000, 1,000 to 20,000,000 |
| `country` | text | 10 countries (meaning not documented; treated as the track's market) |
| `explicit`, `is_explicit_bool` | 0/1, bool | Explicit flag |
| `label` | text | 8 labels |
| `release_year`, `release_month`, `release_day_of_week`, `release_quarter`, `is_weekend_release` | derived | Calendar fields; all consistent with `release_date` |
| `popularity_category` | text | Low (0-32), Medium (33-65), High (66-100) |
| `loudness_category` | text | Quiet (< -30), Moderate (-30 to -10), Loud (> -10) |
| `log_stream_count` | decimal | Exactly `log(1 + stream_count)` |
| `upbeat_score` | decimal | Exactly `(danceability + energy) / 2` (redundant) |
| `artist_track_count` | integer | Tracks under the same `artist_name` |

## Columns added by this project (workbook `Data` sheet)
| Column | Description |
|---|---|
| `breakout_lift` | Streams divided by the streams expected at that popularity (smoothed geometric mean). x10 = ten times expectation |
| `Match` | 1 if the track passes all five dashboard filters |
| `KeyStreams` | Ranking key: matched streams, ties broken by popularity then row (drives Top songs / artists) |
| `KeyPopularity`, `KeyLift` | Ranking keys for the Top-3 popularity and breakout lists |

## Definitions
| Term | Definition |
|---|---|
| Hit | `stream_count` at or above the 99th percentile (5,751,090): 850 tracks |
| Hit rate | Share of tracks in a group that are hits |
| Popularity band | 0-20, 21-35, 36-50, 51-65, 66-80, 81-100 |
