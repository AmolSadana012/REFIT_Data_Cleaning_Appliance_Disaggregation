# Washing-machine EDA findings

## Households and data quality

Selected households: House 1 (Appliance5), House 2 (Appliance2), House 6 (Appliance2), House 9 (Appliance3), House 10 (Appliance5). Appliance mappings come from the supplied dataset documentation; the files report active power in watts.

The source data is recorded at approximately 6-8 second intervals. The table reports the observed median and 95th-percentile interval, date range, quality-flag share, zero time, and event estimates for each chosen home.

 house washing_machine_channel          date_start            date_end  interval_median_seconds  interval_p95_seconds  gaps_over_60_seconds  raw_washing_machine_missing_rows  issues_percent  zero_percent_of_valid_minutes  event_count  median_event_duration_minutes  median_event_peak_power_w
     1              Appliance5 2013-10-09 13:06:17 2015-07-10 11:56:32                      7.0                  14.0                  3440                                 0        0.835962                      97.632573          381                           33.0                2307.000000
     2              Appliance2 2013-09-17 22:08:11 2015-05-28 08:05:43                      7.0                  14.0                  2305                                 0        0.496100                      94.397598          342                           95.0                2088.055542
     6              Appliance2 2013-11-28 12:15:35 2015-06-28 22:14:45                      7.0                  14.0                  5030                                 0        0.551925                      97.451176          123                          114.0                2049.800049
     9              Appliance3 2013-12-17 17:26:20 2015-07-08 18:52:50                      7.0                  13.0                  2764                                 0        0.522342                      90.650210          251                           72.0                2204.666748
    10              Appliance5 2013-11-20 11:31:18 2015-06-30 11:07:07                      7.0                  13.0                  5940                                 0        0.447555                      90.241935          612                          120.0                2051.479126

The official cleaned dataset notes that missing values have been forward-filled, so the original missingness cannot be reconstructed fully from these files. We still count timestamp gaps over 60 seconds after sorting and resample to one-minute means. `Issues == 1` timestamps are excluded from household appliance traces and events; their share is reported above. Household coverage is not identical, and the sample is a small UK study rather than a representative population.

## Event and usage rule

A one-minute reading of at least 20 W is active. Valid off periods of at most 10 consecutive minutes between active runs are joined. Events shorter than 20 minutes are discarded. The resulting 1,709 events have median duration 88.0 minutes and median peak power 2099.0 W. Bridging helps join heating/drum phases, but estimated event boundaries and duration depend on these thresholds.

Zero/off time is reported as the percentage of valid one-minute washing-machine values equal to zero; below-threshold time (<20 W) is also in `household_summary.csv`. Hours with the highest household-averaged mean washing-machine power: 10:00. Weekday with the highest average washing-machine power: Sunday; weekday with the most detected event starts: Sunday.

## Research context

Murray et al. (2017) describe REFIT and its approximately 8-second readings. Khazaei et al. (2020) evaluate simple NILM methods and preprocessing on REFIT and REDD at one-minute resolution, motivating a manageable analysis and common interval. Batra et al. (2014) motivate reproducible evaluation. Our event thresholds are project heuristics, not thresholds claimed from these papers. Full citations: project-root `REFERENCES.md`.

## Plot interpretations

- `average_24_hour_trace.png`: shows the equal-household average aggregate and washing-machine traces at each minute of the day. The washer trace is much smaller than the aggregate, illustrating why identifying it from whole-house load alone is difficult.
- `hour_and_weekday_usage.png`: compares household-averaged power by hour and weekday; the highest hour in this sample is 10:00 and the weekday power peak is Sunday. These are descriptive patterns, not causal or population-wide estimates.
- `example_washing_machine_event.png`: overlays aggregate and washer power for a typical event in House 10, with the detected interval shaded. Other household loads remain visible in the aggregate and can coincide with the washer.
- `cross_household_comparison.png`: compares detected event-duration and peak-power distributions. Differences reflect both household behavior/appliances and the same threshold rule; the boxes are not a controlled comparison.

## Output files

Tables: `household_summary.csv`, `household_comparison.csv`, `detected_events.csv`, `usage_by_hour.csv`, `usage_by_weekday.csv`, `average_24_hour_trace.csv`. Plots: `average_24_hour_trace.png`, `hour_and_weekday_usage.png`, `cross_household_comparison.png`, `example_washing_machine_event.png`.
