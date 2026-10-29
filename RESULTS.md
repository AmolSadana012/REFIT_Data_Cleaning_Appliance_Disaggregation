# Analysis results

These are observed results from the notebooks and the files present in this workspace. They are descriptive and depend on the documented cleaning and event rules.

## Raw House 1 cleaning

- Raw Part 1 contains 4,377,877 rows from 2013-10-09 through 2014-10-01. Sorting reveals 645,514 repeated timestamps (removed by keeping the last row per timestamp); the median interval after deduplication is 5 seconds and the 95th percentile is 15 seconds. The largest observed gap is about 41.6 days.
- Raw Part 2 contains 4,140,886 rows from 2014-06-27 through 2015-07-10, with no duplicate timestamps in the notebook profile. Its median interval is 7 seconds, 95th percentile 8 seconds, and largest gap about 8.7 days. It contains 5,284,083 missing channel values.
- The parts overlap for 137,011 one-minute bins. The final output has 920,031 one-minute timestamps from 2013-10-09 14:06 through 2015-07-10 11:56.
- After interpolation of complete gaps no longer than five minutes, the aggregate still has 119,358 missing/outage minutes; these remain flagged. The cleaned output and per-channel details are in [`outputs/01_data_cleaning/`](outputs/01_data_cleaning/).
- The cleaning plot compares aggregate, House 1 Appliance 5 washing machine, and Appliance 4 washer-dryer readings before and after the documented spike/gap handling. It shows the most active seven-day interval for those appliance channels.

## Washing-machine EDA

The official cleaned dataset provides washer readings for five analyzed homes: House 1 (Appliance 5), House 2 (Appliance 2), House 6 (Appliance 2), House 9 (Appliance 3), and House 10 (Appliance 5). Their date coverage runs from 2013-09-17 to 2015-07-10; observed median sampling interval is 7 seconds, with 95th-percentile intervals of 13-14 seconds. Timestamp gaps over 60 seconds number 2,305-5,940 by household; the washer channels themselves contain no missing rows in these cleaned files, but the original dataset's forward-fill process limits missingness analysis.

The event rule is at least 20 W for a one-minute bin, bridging up to 10 valid below-threshold minutes, with a minimum event length of 20 minutes. It estimates 1,709 events total. Per-house event counts and medians are in `outputs/02_washing_machine_eda/household_summary.csv`; median event durations range from 33 minutes in House 1 to 120 minutes in House 10. Zero time ranges from about 90.2% to 97.6% of valid one-minute washer readings. The observed highest hour by equal-household average power is 10:00; Sunday has the highest weekday average power and the most event starts in this sample.

The supplied cleaned-data notes warn that readings were forward-filled, so missing values in the original collection cannot be reconstructed fully. The analysis excludes rows marked with `Issues == 1`; the flagged share for the five selected homes ranges from 0.45% to 0.84%.

## Disaggregation model

The model uses official cleaned House 1 data at one-minute resolution, with current aggregate power and lags of 1, 2, 5, and 15 minutes. The first 80% of valid chronological feature/target timestamps form training; the last 20% form the test. Training is evenly sampled to 126,228 rows. Both methods use the same 157,786 test timestamps (2015-02-21 20:27 through 2015-07-10 11:56).

| Model | MAE (W) | RMSE (W) | Actual test energy (kWh) | Predicted test energy (kWh) | Absolute energy error (%) |
|---|---:|---:|---:|---:|---:|
| Always-off baseline | 10.479 | 135.070 | 27.558 | 0.000 | 100.000 |
| Random Forest | 12.046 | 102.365 | 27.558 | 24.517 | 11.033 |

The Random Forest has lower RMSE and much lower energy error, but **worse MAE** than the always-off baseline. That trade-off reflects rare high-power washer events and false positive predictions while the washer is off. Two concrete test errors and full test predictions are saved in `outputs/03_disaggregation_model/`.

## Literature context and methodological choices

The REFIT dataset paper documents the approximate 8-second aggregate/appliance data and cleaned dataset context. Khazaei et al. (2020) directly evaluate low-complexity NILM methods and preprocessing on REFIT and REDD after resampling to one minute; this motivates the common one-minute interval and a manageable conventional model, but their Decision Tree/KNN/DBSCAN methods and scores are not reproduced here. NILMTK motivates reproducible preprocessing and explicit baseline/metric reporting. A low-resolution NILM study provides context for the caution about 15- and 30-minute measurements. Breiman's Random Forest paper is cited for the model family, not as a REFIT-specific Random Forest result.

This project independently uses five current/past aggregate features, a chronological 80/20 split, an always-off baseline, and MAE/RMSE plus absolute test-energy error. The Random Forest is a compact regression baseline, not a reproduction of a paper's algorithm, configuration, or benchmark scores. Event threshold and duration settings are transparent project heuristics. Full citations and the distinction between adopted context and original implementation are in [`REFERENCES.md`](REFERENCES.md).
