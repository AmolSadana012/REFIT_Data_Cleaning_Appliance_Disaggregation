# Practical recommendations

## Opportunity and impact measurement

The five-house EDA found the highest equal-household average washer power at 10:00 and the highest weekday mean power/event count on Sunday. A realistic utility opportunity is an opt-in customer insight or time-of-use pilot that helps customers shift flexible washing to lower-carbon or lower-cost periods, without assuming that every household has the same schedule.

Measure the pilot against a randomized or carefully matched control group. Compare peak-window household kWh and the timing of detected washing events before and during the pilot, adjust for season and temperature where relevant, and also track total daily kWh so a shifted load is not mistaken for an energy saving. Use a sample with appliance submeters or high-resolution meter validation to estimate how often the washer event detector is correct.

## Limits of appliance estimates from aggregate power

Whole-house readings combine several appliances. A kettle, heater, dryer, or cooking load may resemble part of a washing-machine cycle, and simultaneous loads can hide a washer signal. Cycle phases vary, and threshold-based events are sensitive to the 20 W threshold, 10-minute bridge, and 20-minute minimum used here. The official cleaned data has forward-filled values and flagged submeter discrepancies. The House 1 Random Forest illustrates this ambiguity: on the same chronological test timestamps, its MAE is 12.046 W versus 10.479 W for always-off, but RMSE is 102.365 W versus 135.070 W, and absolute test-energy error is 11.033% versus 100.000%. It therefore improves RMSE and total-energy estimation but worsens MAE, with both false positives and missed high-power points. The full metric definitions and results are in [`outputs/03_disaggregation_model/model_metrics.csv`](outputs/03_disaggregation_model/model_metrics.csv) and [`RESULTS.md`](RESULTS.md).

## Generalizing from the UK to India

REFIT represents a small UK household cohort and older appliance/metering conditions. Appliance ownership, washing-machine designs and cycles, household size, climate, housing, tariffs, voltage conditions, and time-of-use routines may differ in India. A utility should validate mappings and thresholds on local households, include relevant urban/rural and income groups, and recalibrate with locally measured appliance data before using estimates for customer decisions or planning.

## 15-minute or 30-minute readings

Coarser readings can support broad daily energy and peak-period comparisons, but can average away short washer heating or motor peaks, merge adjacent events, and make start/end times and peak power less reliable. A 15- or 30-minute interval may still be useful for load shifting at that same time scale, but it is a weaker basis for appliance-level cycle detection and should not be presented as precise individual-appliance consumption without validation.

## Interpretation of this project

Under the chosen rule, the five selected households have 1,709 estimated events, with median duration 33-120 minutes by household and median zero/off time about 90-98%. These are rule-dependent estimates from UK data, not a population-wide forecast. The recommended pilot should treat these as hypotheses and measure outcomes with a controlled design.

## Research context

The REFIT dataset description supports the dataset cadence and cohort context; NILMTK motivates reproducible preprocessing and comparison against explicit baselines and metrics; and low-resolution NILM research motivates caution when discussing 15- or 30-minute measurements. These references do not validate this project's event thresholds or imply that its household results generalize to India. See [`REFERENCES.md`](REFERENCES.md) for citations and the scope of each reference.
