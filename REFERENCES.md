# NILM and REFIT references

These references provide context for the REFIT dataset, REFIT-based algorithm/preprocessing evaluation, reproducible NILM comparison, and the limits of lower-resolution appliance disaggregation. They motivate the project design; the results reported here were computed from the local REFIT files and were not copied from the cited papers.

1. Murray, D., Stankovic, L., & Stankovic, V. (2017). “An electrical load measurements dataset of United Kingdom households from a two-year longitudinal study.” *Scientific Data*, 4, 160122. [https://doi.org/10.1038/sdata.2016.122](https://doi.org/10.1038/sdata.2016.122)  
   **Relevance adopted:** documents REFIT's aggregate and appliance channels, approximately 8-second readings, household cohort, and the availability of cleaned measurements. We use the supplied local data and readme, inspect the actual files, and explicitly account for their reported forward filling and issue flags instead of treating them as pristine.

2. Khazaei, M., Stankovic, L., & Stankovic, V. (2020). “Evaluation of low-complexity supervised and unsupervised NILM methods and pre-processing for detection of multistate white goods.” *Proceedings of the 5th International Workshop on Non-Intrusive Load Monitoring*, 34-38. [https://doi.org/10.1145/3427771.3427850](https://doi.org/10.1145/3427771.3427850)  
   **Relevance adopted:** this is the closest REFIT-based algorithm/preprocessing precedent in this review: it evaluates low-complexity Decision Tree, KNN, and DBSCAN methods and preprocessing on REFIT and REDD resampled to one minute. It motivates our manageable model scope and a common one-minute analysis interval. We did not adopt its algorithms, thresholds, or reported performance; our Random Forest is a separate choice and is evaluated on our own chronological House 1 test period.

3. Batra, N., Kelly, J., Parson, O., Dutta, H., Knottenbelt, W., Rogers, A., Singh, A., & Srivastava, M. (2014). “NILMTK: An Open Source Toolkit for Non-intrusive Load Monitoring.” *Proceedings of the 5th ACM International Conference on Future Energy Systems (e-Energy '14)*, 265-276. [https://doi.org/10.1145/2602044.2602051](https://doi.org/10.1145/2602044.2602051)  
   **Relevance adopted:** motivates making preprocessing, evaluation data, baselines, and metrics explicit and reproducible. Here we record source and resampling rules, compare against an always-off baseline, evaluate MAE and RMSE, and calculate a defined test-period energy error. The project does not use NILMTK itself.

4. Liao, J., Elafoudi, G., Stankovic, L., & Stankovic, V. (2014). “Non-intrusive appliance load monitoring using low-resolution smart meter data.” *2014 IEEE International Conference on Smart Grid Communications*, 535-540. [https://doi.org/10.1109/SmartGridComm.2014.7007702](https://doi.org/10.1109/SmartGridComm.2014.7007702)  
   **Relevance adopted:** provides context for discussing appliance estimation when meter readings have lower temporal resolution. The project's 15- and 30-minute discussion is a limitation assessment, not an experiment or a result attributed to this paper.

5. Breiman, L. (2001). “Random Forests.” *Machine Learning*, 45, 5-32. [https://doi.org/10.1023/A:1010933404324](https://doi.org/10.1023/A:1010933404324)  
   **Relevance adopted:** background for choosing Random Forest as a familiar nonlinear regression model. The project uses a small scikit-learn regressor on aggregate power at the current minute and past lags; its fitted parameters and performance are independently evaluated on the chronological REFIT test period.

## What this project does and does not claim

- The Random Forest is a deliberately manageable supervised-regression baseline. Khazaei et al. evaluate Decision Tree, KNN, and DBSCAN rather than this Random Forest; our model is not a reproduction of their algorithms or reported scores.
- The always-off prediction is an explicit reference baseline for the same test timestamps. It reveals why a model can improve RMSE and total-energy estimates while having worse MAE.
- Chronological training/testing and past-only aggregate features are implementation choices to respect time order and avoid future-input leakage; the cited works are context, not a claim that those exact choices or hyperparameters are prescribed by them.
- Appliance events use this project's stated 20 W, 10-minute bridge, and 20-minute minimum-duration rule. These are transparent heuristics, not thresholds claimed to come from the cited research.
