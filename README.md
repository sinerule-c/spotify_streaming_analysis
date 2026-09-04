# Spotify Streaming Analysis

Dataset: https://www.kaggle.com/datasets/beamhonor0911/spotify-artist-streaming-analytics-20202025

<br>

## MySQL Analysis:
### 1. First analysis:
## Top artist
```
SELECT
    artist_name,
    SUM(stream_count) as total_streams,
    ROUND(SUM(stream_count)/
    (SELECT 
		SUM(stream_count) 
        FROM spotify) 
	* 100, 2) AS percentage_streams
FROM spotify
GROUP BY artist_name
ORDER BY total_streams DESC
LIMIT 10;
```
| artist_name | total_streams | percentage_streams |
|---|---|---|
| Knot Hart | 1303217036 | 22.02 |
| IsleEyre | 669929807 | 11.32 |
| Ethereal | 433176636 | 7.32 |
| Sterling Zen | 245065284 | 4.14 |
| Rocco Ore | 219663605 | 3.71 |
| SethStone | 179681824 | 3.04 |
| ElaraRain | 132996215 | 2.25 |
| ValeGlass | 120591894 | 2.04 |
| Frost Nova | 104088727 | 1.76 |
| The Dawns | 99573871 | 1.68 |

**The most streamed artist is Knot Hart**

<br>

## Why is Knot Hart the most streamed artist?
```
SELECT
	artist_name,
    COUNT(*) AS track_count,
    SUM(stream_count) AS total_streams,
    ROUND(AVG(stream_count), 2) AS average_streams_per_track
FROM spotify
GROUP BY artist_name
ORDER BY total_streams DESC
LIMIT 10;
```
| artist_name | track_count | total_streams | average_streams_per_track |
|-|-|-|-|
| Knot Hart | 12111 | 1303217036 | 107606.06 |
| IsleEyre | 5271 | 669929807 | 127097.29 |
| Ethereal | 3232 | 433176636 | 134027.42 |
| Sterling Zen | 2268 | 245065284 | 108053.48 |
| Rocco Ore | 1709 | 219663605 | 128533.41 |
| SethStone | 1486 | 179681824 | 120916.44 |
| ElaraRain | 1111 | 132996215 | 119708.56 |
| ValeGlass | 981 | 120591894 | 122927.52 |
| Frost Nova | 862 | 104088727 | 120752.58 |
| The Dawns | 750 | 99573871 | 132765.16 |

**Even though he has the lowest average_streams_per_track, Knot Hart is the most streamed artist, mainly because he has the most number of tracks.**

<br>

### 2. Second analysis:
## Top track
```
SELECT
	track_name,
    artist_name,
    SUM(stream_count) AS total_streams,
    ROUND(SUM(stream_count)/
        (SELECT
			SUM(stream_count)
            FROM spotify)
		* 100, 2) AS percentage_streams
FROM spotify
GROUP BY track_name, artist_name
ORDER BY total_streams DESC
LIMIT 10;
```
| track_name | artist_name | total_streams | percentage_streams |
|-|-|-|-|
| The Roses	| Grove Nova | 43345015 | 0.73 |
| The Streets | Ethereal | 26699985 | 0.45 |
| Bitter | Flora Quinn | 26635547 | 0.45 |
| Islands | Knot Hart | 20682255 | 0.35 |
| Breaking Thunder | SethStone | 20408155 | 0.34 |
| The Pages | IsleEyre | 19926997 | 0.34 |
| Storm | Knot Hart | 18040424 | 0.30 |
| Wings | IsleEyre | 17644367 | 0.30 |
| Lights | Dean System | 17250422 | 0.29 |
| Lights | Knot Hart | 16873308 | 0.29 |

**The most streamed song is The Roses by Grove Nova**

<br>

** Lets investigate what distinguishes 'The Roses' by Grove Nova from other tracks
First retrieving its full detail:
```
SELECT *
FROM spotify
WHERE artist_name = 'Grove Nova'
	AND track_name = 'The Roses'
ORDER BY stream_count DESC;
```
| track_id | track_name | artist_name | album_name | release_date | genre | duration_ms | popularity | danceability | energy | key | loudness | mode | instrumentalness | tempo | stream_count | country | explicit | label | release_year | release_month | release_day_of_week | duration_minutes | popularity_category | loudness_category | key_name | mode_name | is_explicit_bool | release_quarter | is_weekend_release | log_stream_count | upbeat_score | artist_track_count |
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
| c7a7d73d-9531-4bdc-b195-ea7d81b9a028 | The Roses | Grove Nova | Frozen Flames | 2024-04-29 | Alternative | 228188 | 90 | 0.6297 | 0.6017 | 6 | -8.61 | 1 | 0.412 | 151.59 | 43345015 | IN | True | Columbia Records | 2024 | 4 | Monday | 3.8 | Very High | Moderate | F# | Major | True | Q2 | False | 17.5847 | 0.6234 | 124 |

<br>

**Investigating whether release date affects the stream count**
```
SELECT
	DATE_FORMAT(release_date, '%b %Y') AS month_year,
    COUNT(*) AS track_count,
    ROUND(AVG(stream_count), 2) AS average_streams
FROM spotify
WHERE release_date >= '2024-04-01'
	AND release_date < '2024-05-01';
```

| month_year | track_count | average_streams |
|-|-|-|
| Apr 2024 | 676 | 200843.63 |

**The Roses has 43,345,015 streams while songs released in April 2024, on average has 200,843 streams. This tells us release date alone cannot explain why it is the most streamed track.**
