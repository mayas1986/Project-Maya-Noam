# SOURCE.md - Project Proposals

## Proposal 1: Predicting Ahuzot HaHof Parking Availability in Tel Aviv

**Preference:** 1 - our first choice.

**Research question and description:** Can we predict which Ahuzot HaHof parking garages in Tel Aviv will be full at a given time? We plan to collect live availability snapshots for the garages, including garage name, identifier, address, availability status and status time. We will examine patterns by hour and weekday, potentially alongside weather conditions.

**Sources and URLs:**
- Tel Aviv municipality's live GIS/API layer (Ahuzot HaHof garages, layer 970): https://gisn.tel-aviv.gov.il/ArcGIS/rest/services/IView2/MapServer/970
- Official Ahuzot HaHof parking directory: https://www.ahuzot.co.il/Parking/All/
- Open-Meteo weather API: https://open-meteo.com/en/docs

**Update frequency:** Garage statuses are updated live (each record carries its own status timestamp). We plan to collect a snapshot every 30 minutes. Weather will be matched hourly.

**Estimated size per sample:** About 5-30 KB of filtered JSON per snapshot. At 48 snapshots a day, this stays well below 500 MB for the semester.

**Authentication:** None. The municipal ArcGIS layer offers a public JSON query interface with no API key, and Open-Meteo needs no key for non-commercial use.

**Data sample (selected fields from a live municipal API response, November 7, 2026):**

Query: `https://gisn.tel-aviv.gov.il/ArcGIS/rest/services/IView2/MapServer/970/query?where=1%3D1&outFields=*&returnGeometry=false&f=json`

```json
{
  "code_achoza": 11,
  "shem_chenyon": "תל - נורדאו",
  "ktovet": "פרישמן 28 תל-אביב יפו",
  "lat": 32.0797,
  "lon": 34.7718,
  "status_chenyon": "מלא",
  "tr_status_chenyon": 1794066756240
}
```

`status_chenyon` is the garage status (`פנוי` available, `מעט` limited, `מלא` full, `סגור` closed), and `tr_status_chenyon` is the status timestamp (Unix epoch, milliseconds).

**Why collect it ourselves?** The API returns only current parking statuses and does not provide historical snapshots. Our repeated snapshots will create a new historical dataset for predicting availability. We will confirm that the municipal data's terms of use allow this collection.

## Proposal 2: Predicting Netflix Top 10 Success

**Preference:** 2 - our second choice.

**Research question and description:** Can the popularity of a TV series on TMDB help predict whether it will enter Netflix's weekly Top 10? We plan to save daily snapshots of trending series (title, ID, position and popularity), then compare them with Netflix's official weekly TV rankings, matching the same series across sources.

**Sources and URLs:**
- TMDB Trending TV API: https://api.themoviedb.org/3/trending/tv/day
- TMDB documentation: https://developer.themoviedb.org/reference/trending-tv
- Netflix official Top 10: https://www.netflix.com/tudum/top10
- Netflix global rankings file: https://www.netflix.com/tudum/top10/data/all-weeks-global.tsv

**Update frequency:** TMDB trending is updated daily, we will collect once a day. Netflix publishes the Top 10 weekly and we will collect once a week.

**Estimated size per sample:** About 10-30 KB for one page of TMDB JSON, and 1-3 MB for the complete Netflix global rankings file. Total under 500 MB.

**Authentication:** A free TMDB API key is required (non-commercial use with attribution). The Netflix Top 10 file is public and needs no login. API keys will not be committed to the public repository.

**Data sample (row from Netflix's official `all-weeks-global.tsv`):**

```
week        category      weekly_rank  show_title  season_title           weekly_hours_viewed  runtime  weekly_views  cumulative_weeks_in_top_10
2026-10-18  TV (English)  1            Below       Below: Limited Series  86400000             4.0667   21200000      2
```

For the week ending October 18, 2026, *Below: Limited Series* ranked #1 in English TV with 21,200,000 views.

**Why collect it ourselves?** Netflix already publishes past Top 10 lists, but TMDB's trending list is rolling and not archived. Our time-stamped history of TMDB trending, linked to later Netflix results, is the new data.
