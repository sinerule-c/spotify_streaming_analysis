# Spotify Streaming Analysis

Dataset: https://www.kaggle.com/datasets/beamhonor0911/spotify-artist-streaming-analytics-20202025

<br>

## MySQL Analysis:
### First analysis:
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

*The most streamed artist is Knot Hart*

<br>

### Second analysis:
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

*The most streamed song is The Roses by Grove Nova*

<br>


