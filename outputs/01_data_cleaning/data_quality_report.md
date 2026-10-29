# House 1 raw data quality and cleaning report

## Inputs and cleaning rules

Inputs are the existing `RAW_House1_Part1.csv` and `RAW_House1_Part2.csv` files. Input files were not modified.
Within each file, invalid timestamps are dropped, duplicate timestamps keep the last row in file order, and rows are then sorted by timestamp. Minute means are calculated per file; where both parts contain a minute, Part 2 replaces Part 1 for that whole minute, including explicit missing values. Values above 4,000 W are treated as spikes on all channels. Complete internal gaps up to five minutes are time-interpolated; longer missing runs are retained and flagged by channel.

## Source profile

| source_file | input_rows | invalid_timestamp_rows | duplicate_timestamps_removed | rows_after_timestamp_deduplication | start | end | interval_min_seconds | interval_median_seconds | interval_p95_seconds | interval_max_seconds | gaps_over_60_seconds | raw_missing_values | values_over_4000_watts |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| RAW_House1_Part1.csv | 4377877 | 0 | 645514 | 3732363 | 2013-10-09 14:06:17 | 2014-10-01 01:27:07 | 1.0 | 5.0 | 15.0 | 3594413.0 | 780 | 0 | 19647 |
| RAW_House1_Part2.csv | 4140886 | 0 | 0 | 4140886 | 2014-06-27 21:57:50 | 2015-07-10 11:56:32 | 1.0 | 7.0 | 8.0 | 747252.0 | 3603 | 5284083 | 21505 |

The two source parts overlap for 137,011 minute bins. The final one-minute series contains 920,031 timestamps from 2013-10-09 14:06:00 through 2015-07-10 11:56:00.

## Missing readings and long outages

| channel | missing_minutes_before_fill | short_gap_minutes_filled | long_outage_minutes_flagged | missing_minutes_after_fill |
| --- | --- | --- | --- | --- |
| Aggregate | 122035 | 2677 | 119358 | 119358 |
| Appliance1 | 123780 | 2797 | 120983 | 120983 |
| Appliance2 | 123469 | 2368 | 121101 | 121101 |
| Appliance3 | 121140 | 929 | 120211 | 120211 |
| Appliance4 | 132846 | 3242 | 129604 | 129604 |
| Appliance5 | 129693 | 3578 | 126115 | 126115 |
| Appliance6 | 120559 | 839 | 119720 | 119720 |
| Appliance7 | 119701 | 895 | 118806 | 118806 |
| Appliance8 | 121885 | 1856 | 120029 | 120029 |
| Appliance9 | 338428 | 1062 | 337366 | 337366 |

## Interpretation and caveats

House 1 Part 1 is not chronologically ordered and contains repeated timestamps; Part 2 contains explicit NaNs and overlaps Part 1. The source interval statistics and missing counts above describe the observed data rather than assuming a perfectly regular 6–8-second cadence. The official cleaned-data notes specifically describe >4,000 W IAM spikes; applying this threshold to the aggregate as well is a conservative analysis choice and may remove a genuinely unusual high household load. The input data only records active power, and the sensors are not perfectly synchronized.

The figure compares raw and cleaned one-minute means over the seven-day period with the greatest combined activity on the washer-dryer and washing machine (2015-02-02 to 2015-02-08). Plot limits use the 99.9th percentile to keep normal variation visible, so the y-axis may omit the most extreme raw spikes.

## Outputs

- `house1_cleaned_1min.csv.gz`: cleaned one-minute power channels plus per-channel long-outage flags.
- `house1_before_after_cleaning.png`: aggregate, washing-machine, and washer-dryer traces before and after cleaning.
- `source_quality_summary.csv`, `channel_quality_by_source.csv`, and `channel_gap_summary.csv`: tabular quality findings.
