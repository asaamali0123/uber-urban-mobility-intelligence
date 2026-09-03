# Uber Urban Mobility & Demand Intelligence — NYC FHVHV

An end-to-end portfolio analysis of **20.55M cleaned NYC high-volume for-hire vehicle trips from January 2026**, using memory-efficient Python processing, descriptive statistics, geographic/economic analysis, and Power BI.

## Project objective

This project answers practical mobility questions:

- When is ride demand highest?
- How do peak and non-peak trips differ?
- Which zones and boroughs generate the most activity?
- How do riders move across taxi zones and boroughs?
- How do fare and driver-pay patterns change by time, distance, and airport?
- What service-behavior patterns appear in shared/WAV/Access-A-Ride flags?

## Dataset

**Source:** NYC TLC High Volume For-Hire Vehicle trip records, January 2026.

| Stage | Rows | Columns |
|---|---:|---:|
| Raw | 20,940,373 | 25 |
| Cleaned analytical dataset | 20,553,794 | 26 |

The raw Parquet file is several gigabytes, so it is **not included in this repository**.

## Tech stack

- Python
- Pandas
- NumPy
- PyArrow / Parquet
- Matplotlib
- Seaborn
- Jupyter Notebook
- Power BI

## Memory-efficient approach

The project avoids loading the full 20M-row dataset into Pandas at once.

Instead:

1. Read Parquet row groups/chunks.
2. Audit and clean each chunk.
3. Apply metric-specific filters.
4. Aggregate compact results.
5. Export small analytical CSVs for Power BI.

This keeps the workflow usable on limited-memory hardware.

## Data-quality strategy

The raw data included negative fares, zero-distance/time trips, and invalid timestamp sequences.

A key principle in this project is **metric-specific cleaning**:

- zero-mile trips can still count toward demand;
- distance metrics require `trip_miles > 0`;
- speed requires both positive distance and positive trip time;
- negative driver-pay records are excluded from driver-pay metrics;
- special outside-NYC location records are not dropped just because borough fields are blank.

## Key findings

### Demand
- Peak request hour: **18:00**, about **1.26M trips**
- Friday and Saturday evenings show the strongest demand concentration.
- Saturday 16:00–23:00 represents about **46.48%** of Saturday trips.

### Trip performance
- Average distance: **4.54 miles**
- Average duration: **18.54 minutes**
- Aggregate average speed: **14.70 mph**
- Peak aggregate speed: **13.34 mph**
- Non-peak aggregate speed: **15.28 mph**

### Geography
- Top pickup zone: **LaGuardia Airport**
- Second: **JFK Airport**
- Airport pickups together: about **3.33%** of all cleaned trips
- Largest pickup borough: **Manhattan**
- **91.01%** of trips cross a taxi-zone boundary
- **73.52%** remain within the same borough

### Airport economics

| Airport | Avg distance | Avg duration | Avg fare | Avg driver pay |
|---|---:|---:|---:|---:|
| LaGuardia | 11.23 mi | 31.87 min | $55.54 | $39.89 |
| JFK | 17.74 mi | 42.68 min | $76.01 | $55.40 |

LGA has more pickup volume, while JFK trips are longer and higher-value on average.

### Trip segments
- **41.14%** of positive-distance trips are 1–3 miles.
- **54.51%** are 3 miles or less.
- **43.81%** of positive-duration trips are 5–15 minutes.
- **79.41%** are 5–30 minutes.

### Service behavior
- Shared request rate: **1.89%**
- Shared match flag: **1.11%**
- Access-A-Ride: **0.09%**
- WAV request: **0.19%**
- WAV match flag: **9.11%**

Among explicit shared requests, about **58.22%** were recorded as matched.

## Statistical analysis

Pearson correlations:

| Relationship | r |
|---|---:|
| Distance vs Duration | 0.783 |
| Distance vs Fare | 0.840 |
| Distance vs Driver Pay | 0.901 |
| Duration vs Fare | 0.795 |
| Duration vs Driver Pay | 0.903 |
| Fare vs Driver Pay | 0.930 |

All are interpreted as **association, not causation**.

## Power BI dashboard

The final report has three pages:

1. **Executive Overview** — KPIs, hourly demand, day-hour intensity, peak demand.
2. **Demand & Mobility** — top zones, borough flows, mobility structure, airport economics.
3. **Economics & Behavior** — peak vs non-peak performance, economics, trip segments and service usage.

## Repository structure

```text
uber-urban-mobility-intelligence/
├── README.md
├── requirements.txt
├── reports/
│   ├── Uber_Urban_Mobility_Public_Portfolio_Report.pdf
│   └── Uber_Urban_Mobility_Detailed_Learning_Report.pdf
├── notebooks/
│   └── Uber_operation_analysis.ipynb
├── powerbi/
│   └── Uber_Urban_Mobility_Intelligence.pbix
├── powerbi_data/
│   └── aggregate CSV files
└── images/
    └── dashboard screenshots
```

## Reproduce the analysis

1. Download the January 2026 NYC TLC FHVHV Parquet file and taxi-zone lookup.
2. Update local file paths in the notebook if required.
3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Run the notebook.
5. Use the generated aggregate CSVs to refresh/build the Power BI report.

## Important interpretation notes

- Peak = analyst-defined **16:00–20:00** exploratory window.
- Driver pay per passenger-trip-hour is **not actual online hourly earnings**.
- Cross-zone movement does **not** automatically mean long-distance travel.
- Outlier flags are investigated rather than deleted automatically.
- WAV fields should not be treated as a simple request-to-match funnel without verifying field semantics.

## Portfolio skills demonstrated

Large-data handling · Data cleaning · Feature engineering · EDA · Statistics · Temporal analysis · Geographic analysis · Operational economics · Power BI · Business communication
