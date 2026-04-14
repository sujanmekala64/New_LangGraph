# Write a blog on 2011 World cup

## 2️⃣ Core Concepts (Data & Domain)

The 2011 World Cup analysis hinges on a clear data model that mirrors cricket semantics and exposes the metrics that matter.

- **Entity Breakdown**  
  * `Match`: `match_id`, `overs`, `venue`, `result`, `toss` (enum).  
  * `Batting`: `runs`, `balls_faced`, `wickets_out` per over; aggregate `batting_avg`.  
  * `Bowling`: `wickets`, `runs_conceded`, `economy = runs_conceded/overs`, `dot_balls`.  
  * `Player`: `role` (batsman, bowler, all‑rounder), innings count, cumulative stats per season.

- **Key Metrics**  
  * **Run‑Rate** = `total_runs / overs` (ov per 5‑ball segment for format).  
  * **Strike‑Rate** = `runs * 100 / balls_faced`.  
  * **Wicket‑Fall Curves** – compute the time (over, ball) of each wicket and plot the CDF: `F(t) = (# wickets fallen by time t) / total_wickets`.  
  * **Fielding Impact** – total `catches + run‑outs` per match normalized by overs bowled.

- **Data Sources**  
  * ESPNcricinfo API – JSON via `/stats` endpoints.  
  * CricketArchive – CSV dumps for historical completeness.

- **Edge Cases**  
  * Weather‑abandoned matches: flag `no_result` and exclude from run‑rate.  
  * Extras (leg byes, wides) must be summed into `total_runs`; otherwise strike‑rate skews.

- **Performance Trade‑offs**  
  * In‑memory processing suffices for ~10 k rows; beyond, stream with `chunksize=5000`.  
  * Create a composite index on `match_id` and `team_id` to accelerate joins.

*Word count:* ~220 words.
