# REFIT Data Cleaning and Appliance Disaggregation 

This project analyzes the REFIT files already present in `data/`. The source datasets and documentation are inputs only; the notebooks save derived results under `outputs/`.

## Run the analysis

From the project root in PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m jupyter lab
```

Open and run the notebooks in order:

1. [`notebooks/01_data_cleaning.ipynb`](notebooks/01_data_cleaning.ipynb) — raw House 1 quality checks and one-minute cleaning.
2. [`notebooks/02_washing_machine_eda.ipynb`](notebooks/02_washing_machine_eda.ipynb) — official cleaned data, five-house washing-machine EDA.
3. [`notebooks/03_disaggregation_model.ipynb`](notebooks/03_disaggregation_model.ipynb) — chronological House 1 always-off baseline and Random Forest.

The notebooks find the existing project root relative to the notebook's working directory and create their output folders automatically. The source CSVs are large; the cleaning and EDA notebooks load one source file at a time.

## Results

- [`outputs/01_data_cleaning/`](outputs/01_data_cleaning/) — cleaned one-minute House 1 data, plots, and quality tables/report.
- [`outputs/02_washing_machine_eda/`](outputs/02_washing_machine_eda/) — event estimates, household comparisons, usage summaries, and plots.
- [`outputs/03_disaggregation_model/`](outputs/03_disaggregation_model/) — test predictions, metrics, failure examples, and actual-versus-predicted plot.
- [`outputs/04_practical_recommendations/`](outputs/04_practical_recommendations/) — practical recommendation summary.
- [`RESULTS.md`](RESULTS.md) — measured findings and model metrics.
- [`DATA.md`](DATA.md) — detected input layout, schemas, and cleaning notes.
- [`Recommendation.md`](Recommendation.md) — practical recommendations and limitations.
- [`REFERENCES.md`](REFERENCES.md) — research and dataset citations, with the project choices they motivate.

See [`DATA.md`](DATA.md) for the existing dataset filenames and [`RESULTS.md`](RESULTS.md) for the observed results. Literature is used as methodological context; all reported project metrics are calculated from the local data. Raw data is not redistributed or downloaded by this project.
