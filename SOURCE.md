# Data Source and Schema

## 1. Source Information

| Property | Details |
|---|---|
| Source name | Tel Aviv Municipality - Ahuzot HaHof Parking Garages |
|Source URL | http://gisn.tel-aviv.gov.il/ArcGIS/rest/services/IView2/MapServer/970 |
| Data format | JSON (ArcGIS REST API) |
| Authentication | No API key or login required |
| License | Tel Aviv Municipality information Use License: free, non-exclusive use, subject to attribution and license conditions. No separate license is specified for GIS layer 970. |
| Planned collection frequency | Every 30 minutes |

## 2. Data Collection Description

We collect JSON snapshots from the Tel Aviv Municipality's ArcGIS API. Each snapshot contains information about Ahuzot HaHof parking garages,
including their identifiers, names, addresses, geographic locations, reported parking statuses,
status timestamps, parking tariffs, and tariff-related notes.

The collected snapshots will be used to build a historical dataset, analyze parking availability patterns over time,
and investigate whether we can predict which garages will be full at a given time. 

## 3. Dataset Schema

Each row in dataset.csv will represent one parking garage in one collected snapshot.

| Column | Description | Data Type / Units |
|---|---|---|
| code_achoza | Unique parking garage code | Integer |
| shem_chenyon | Parking garage name | Text |
| ktovet | Parking garage address | Text |
| lat | Geographic latitude | Decimal degrees |
| lon | Geographic longitude | Decimal degrees |
| status_chenyon | Reported parking garage status | Text; categorical (may be empty) |
| tr_status_chenyon | Status update timestamp | Unix timestamp; milliseconds |
| taarif_yom | Daytime parking tariff information | Text; prices in ILS |
| chalon_taarif_yom | Additional daytime tariff details and time conditions | Text; hours and prices where applicable |
| taarif_layla | Nighttime parking tariff information | Text; prices in ILS |
| chalon_zman_taarif_layla | Nighttime tariff details and applicable times | Text; hours and prices where applicable |
| hearot_taarif | Additional tariff notes and parking conditions | Text; no fixed units |
| collected_at_utc | Date and time when the snapshot was collected | UTC datetime; ISO 8601 |

## 4. Column Selection

We selected the garage code, name, address, and coordinates to identify and locate each parking garage.
We kept the reported status and its timestamp because they are important for our research question.
We also included parking tariffs and tariff notes to examine whether pricing and parking conditions are related to garage availability.
The collected_at_utc column will be derived from each raw file's date and time, allowing us to compare snapshots collected at different times.
We excluded additional technical identifiers because code_achoza is sufficient to identify each garage.
We also excluded x and y because we already have latitude and longitude.
Additional tariff fields and the opening-hours field were excluded because they were empty in the inspected snapshot.
We excluded the parking capacity fields because almost all their values were zero, providing little useful information in this sample.
We also excluded date_import because it was empty, and UniqueId because it had the same value for every garage in the sample.

## 5. Field Name Mapping

| Original Field | Dataset Column |
|---|---|
| code_achoza | code_achoza |
| shem_chenyon | shem_chenyon |
| ktovet | ktovet |
| lat | lat |
| lon | lon |
| status_chenyon | status_chenyon |
| tr_status_chenyon | tr_status_chenyon |
| taarif_yom | taarif_yom |
| chalon_taarif_yom | chalon_taarif_yom |
| taarif_layla | taarif_layla |
| chalon_zman_taarif_layla | chalon_zman_taarif_layla |
| hearot_taarif | hearot_taarif |
| File date and time (UTC) | collected_at_utc |

The original field names will be kept unchanged in dataset.csv.
The collected_at_utc column will be derived from the date and time in each raw file's path.

## 6. Estimated Storage Size

We plan to collect one snapshot every 30 minutes, which means 48 snapshots per day. Automated collection is expected to run from semester week 6 through the end of week 12 (7 weeks total). At this frequency, we expect approximately 2,352 automated snapshots (48 × 7 × 7). An inspected JSON snapshot is about 121 KB, so the estimated raw size for automated collection is 121 KB × 2,352 ≈ 285 MB (decimal units). To estimate the size of dataset.csv, we considered only the 13 columns selected for our dataset, giving an estimated 41 KB per snapshot, or approximately 96 MB over the same period. The four manually collected initial snapshots will add a small amount to these estimates. Actual sizes may vary with the data, and partial weeks or interruptions may reduce the final snapshot count.


