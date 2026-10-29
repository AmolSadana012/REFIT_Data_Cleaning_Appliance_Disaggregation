# Practical recommendation summary

**Opportunity:** test opt-in feedback or time-of-use prompts that move flexible washing away from utility peak or high-carbon hours.

**Measure impact:** compare peak-window household kWh and detected washer-event timing in a randomized or matched control pilot; track total daily kWh as well. Validate appliance-level claims with a submetered sample.

**Cautions:** aggregate readings cannot uniquely identify appliances; this project's event boundaries depend on a threshold rule. REFIT covers a small UK cohort, so thresholds and behavior should be validated with Indian households and local appliances.

**Model result:** on the same test timestamps, Random Forest MAE was 12.046 W versus 10.479 W for always-off; RMSE was 102.365 W versus 135.070 W, and absolute test-energy error was 11.033% versus 100.000%. Better RMSE/energy error does not mean better MAE.

**Meter intervals:** 15- or 30-minute readings can support broad load shifting, but blur appliance peaks and cycle start/end times. They are not reliable evidence of precise appliance-level power without validation.

See the project-root [`Recommendation.md`](../../Recommendation.md) for the full discussion.
