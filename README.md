# Spotify Streaming Analysis

## Dataset: 
https://www.kaggle.com/datasets/beamhonor0911/spotify-artist-streaming-analytics-20202025

### First analysis: Finding the top 10 artists streaming
```
SELECT
    artist_name,
    SUM(stream_count) as total_streams,
    ROUND(SUM(stream_count)/(
    SELECT 
		SUM(stream_count) 
        FROM spotify) 
	* 100, 2) AS percentage_stream_count
FROM spotify
GROUP BY artist_name
ORDER BY total_streams DESC
LIMIT 10;
```
| artist_name | total_streams | percentage_stream_count |
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
