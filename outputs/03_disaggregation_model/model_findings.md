# House 1 washing-machine disaggregation results

## Data and validation design

Official cleaned input: `data/cleaned/CLEAN_REFIT_081116/CLEAN_House1.csv`; target: Appliance 5 washing machine, in watts. The one-minute test uses 157,786 valid timestamps from 2015-02-21 20:27:00 through 2015-07-10 11:56:00. 58,183 rows with `Issues == 1` were excluded before resampling.
Features are current aggregate power and aggregate lags at 1, 2, 5, 15 minutes. The first 80% of complete chronological feature/target timestamps form the training period; the last 20% form the test period. Training was evenly thinned to 126,228 rows (at most 150,000) for laptop-scale fitting. No future aggregate values or washing-machine target lags are used.

## Test metrics

              model  test_timestamps  mae_w  rmse_w  actual_energy_kwh  predicted_energy_kwh  absolute_energy_error_kwh  absolute_energy_error_percent
Always-off baseline           157786 10.479 135.070             27.558                 0.000                     27.558                        100.000
      Random Forest           157786 12.046 102.365             27.558                24.517                      3.040                         11.033

Energy error is defined as `abs(predicted test kWh - actual test kWh) / actual test kWh * 100`; energy for one-minute samples is `sum(power_W) / 60,000`. Metrics are computed on the same test timestamps for both methods. The always-off predictor is a meaningful baseline because the washer is at zero for much of the time.

## Literature context and implementation scope

Khazaei et al. (2020) provide a directly REFIT-based precedent for comparing simple NILM methods and preprocessing at one-minute resolution; their tested Decision Tree, KNN, and DBSCAN methods are not the Random Forest used here. NILMTK (Batra et al., 2014) motivates explicit baselines and multiple accuracy metrics; Breiman (2001) is the general Random Forest reference. Current/past aggregate lags and the chronological split are this project's leakage-conscious choices. This compact model is not a reproduction of a published NILM method or score. Full citations: project-root `REFERENCES.md`.

## Observed failure cases

1. At 2015-04-23 11:25:00, the washer target is 0.0 W while the model predicts 2243.5 W (aggregate 2544.4 W). Other household loads can produce an aggregate pattern that resembles washer activity.
2. At 2015-04-15 13:26:00, actual washer power is 2428.3 W while the model predicts 49.0 W (aggregate 2902.3 W). Similar aggregate totals can come from different appliance combinations, and the model's tree averages smooth sharp washer transitions.

These examples are selected from the actual test residuals and are saved with their readings in `failure_examples.csv`. Appliance-level ambiguity is unavoidable from one aggregate meter; coincident loads and changing cycle phases can make different washer states look similar at the whole-house level.

## Plot and outputs

`actual_vs_predicted.png` shows a three-day test window around the day with the highest actual washer energy. The Random Forest MAE is 12.0 W versus 10.5 W for always-off; its test-period absolute energy error is 11.0%.
`model_metrics.csv` contains MAE, RMSE, and the defined energy metric. `test_predictions.csv.gz` contains actual, baseline, Random Forest, and aggregate readings at all test timestamps. `feature_importance.csv` contains fitted feature importances; `failure_examples.csv` records the two example errors.
