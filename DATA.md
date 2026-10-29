# Data inventory and handling

All analysis uses the files already present under `data/`. The notebooks do not download, move, modify, or overwrite those input files.

## Detected inputs

| Category | Existing location | Files and format |
|---|---|---|
| Raw REFIT | `data/raw/REFIT_RAW_081116/` | CSVs split into `RAW_HouseN_Part1.csv` and `RAW_HouseN_Part2.csv`; Houses 1-12 use this pattern, while House 13 and later households use names such as `RAW_House_13_Part1.csv`. |
| Official cleaned REFIT | `data/cleaned/CLEAN_REFIT_081116/` | One CSV per available household, named `CLEAN_House1.csv` through `CLEAN_House13.csv` and `CLEAN_House15.csv` through `CLEAN_House21.csv`. |
| Dataset documentation | `data/documentation/CLEAN_READ_ME_081116.txt` | Original text readme with file descriptions, appliance mappings, and cleaning notes. |

Both datasets are CSV. Raw columns include `Time`, `Unix`, `Aggregate`, and `Appliance1` through `Appliance9`; cleaned files also include `Issues`. The supplied documentation identifies power readings as active power in watts, collected at approximately 6-8 second intervals. Appliance numbers differ by household; the notebooks use the mappings in the original readme rather than assuming a common channel.

## Source limitations recorded in the supplied readme

- The raw parts are different database iterations: Part 1 represents unavailable sensors as zero, while Part 2 uses `NaN`.
- The cleaned dataset has already forward-filled missing values, made some IAM readings above 4,000 W zero, corrected daylight-saving time, and added `Issues` for submeter/aggregate discrepancies. Original missingness cannot be fully recovered from the official cleaned data.
- Data includes periods of sensor and household outages; polling is not perfectly synchronized across sensors.
- The supplied documentation describes the cohort and licensing. Consult `data/documentation/CLEAN_READ_ME_081116.txt` for the original details and citation. The peer-reviewed REFIT dataset description is listed in [`REFERENCES.md`](REFERENCES.md).

## Analysis handling

- **Raw House 1:** duplicates within each raw part keep the last row in file order; timestamps are sorted; each file is resampled to one-minute means. In minute bins present in both parts, Part 2 takes precedence, preserving its explicit missing values. Values above 4,000 W are marked missing on all power channels. This follows the documented IAM spike guidance and applies that cutoff conservatively to aggregate power as well.
- **Gap handling:** only complete, internal gaps of at most five minutes are time-interpolated. Longer missing runs remain missing and are flagged per channel in the derived CSV.
- **Official cleaned EDA/model inputs:** the documented `Issues == 1` rows are excluded from washer/aggregate analysis. Hour and weekday summaries use the file's naive local timestamps.
- Generated analysis files belong under `outputs/`; the input data directories are excluded from version control by `.gitignore`.
