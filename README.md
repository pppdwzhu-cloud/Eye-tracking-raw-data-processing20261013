# Cookie eye-tracking data processing

JupyterLab code for constructing eye-tracking variables from Tobii Pro Lab exports for a Single versus Multiple cookie-display study, merging survey responses, and exporting labeled SPSS datasets.

## Contents

- `cookie_eye_tracking_analysis.ipynb`: the complete processing and export pipeline.
- `requirements.txt`: dependency versions used in the verified local environment.
- `.gitignore`: excludes local inputs, generated datasets, and notebook checkpoints from Git tracking.

This repository contains code and documentation. Input data must be supplied locally; participant-level inputs and generated datasets are not distributed here. The notebook does not perform the ANOVA or mediation analyses.

## Run in JupyterLab

1. Download or clone this repository and open its folder in Terminal.
2. Activate your Python environment and install the dependencies:

   ```bash
   python -m pip install -r requirements.txt
   ```

3. Supply authorized inputs using the folder layout below, or edit `DATA_DIR` and `SURVEY_PATH` in the first code cell.
4. Launch `jupyter lab` from the repository folder and open the notebook.
5. Choose **Restart Kernel and Run All Cells**. Exported files are written to `OUTPUT_DIR`, which defaults to the repository's `outputs/` folder.

The validation environment uses Python 3.14.4. The notebook reports package versions at runtime. The data-processing pipeline requires the case-sensitive file and column names represented in the original Tobii exports.

All code cells were executed in order in a clean Python process using the original authorized inputs. The final export exactly reproduced the existing 156-record, 103-variable visits dataset, including its stored values and variable metadata. A live Jupyter-kernel test was unavailable in the validation environment because local TCP binding was blocked.

## Local input layout

```text
data/
├── eye_tracking/
│   ├── Group 1_CookieSingle/
│   │   ├── Cookiesingle AOI-based.tsv
│   │   ├── Cookiesingle Interval-based.tsv
│   │   ├── Cookiesingle Data export.tsv
│   │   └── TotalFix_Visit/
│   │       ├── Cookiesingle Fixation_Whole Image.tsv
│   │       └── Cookiesingle Visit.tsv
│   └── Group 2_CookieMultiple/
│       ├── Cookiemultiple AOI-based.tsv
│       ├── Cookiemultiple Interval-based.tsv
│       ├── Cookiemultiple Data export.tsv
│       └── TotalFix_Visit/
│           ├── Cookiemultiple Fixation_Whole Image.tsv
│           └── Cookiemultiple Visit.tsv
└── survey/
    └── FinalDataset_ToMergeWithEyeTracking20261007.sav
```

The first cell checks for all ten TSV files and the survey SAV before starting. It creates the output directory automatically. The expected recording count is derived from the interval exports rather than fixed at 156.

## Definitions and interpretation

- **Conditions and AOIs:** 1 = Single; 2 = Multiple. The same five non-overlapping positional AOIs are used in both conditions. The Single display has four empty positional regions.
- **Units:** native summary durations are milliseconds, raw timestamps are microseconds, and final durations are seconds. Count rates use stimulus viewing time as the denominator.
- **Saccade reconstruction:** consecutive samples sharing an event type/index are collapsed. A reconstructed count requires an immediate Fixation → Saccade → Fixation sequence, known fixation locations, and consistent binary AOI membership. Inter-AOI counts connect different positions; within-AOI counts stay in one position; AOI–background counts include both directions.
- **Omitted saccades:** background–background movements are omitted. Native total minus the three retained categories also contains differences in event eligibility and classification; it should not be interpreted as an exact background–background count.
- **Whole-fixation totals:** the current fixation-summary inputs are interval-based Number_of_whole_fixations and Total_duration_of_whole_fixations. They exclude partial fixations; AOI fixation metrics include partial fixations. AOI/whole-fixation ratios can therefore exceed 1 and are not bounded proportions of all fixation activity. The scientific definitions and calculations are preserved from the original analysis.
- **Visits:** native per-AOI counts and durations are imported. Native averages are retained, including their export rounding. Zero visits have zero total duration and a missing average. Total visits sum counts across the five individual AOIs; this is not the number of visits to their union.
- **Revisits:** subtract one separately for each AOI and clip at zero, then sum for total revisits. The center measure is labeled AOI0.
- **Combined visit duration:** AOIVisS sums total visit durations across positions; AOIVisAv divides that duration by total visits, weighting each visit equally.
- **Coverage:** an AOI is fixated when its native fixation count is greater than zero. Coverage sums these flags across five positions.
- **Missing ratios:** zero denominators produce missing values.
- **Survey join:** Recording = ExperimentID after whitespace/dash normalization in temporary keys. Original IDs, variable definitions, value labels, missing-value rules, and measurement settings are retained. No comprehension-check exclusion is applied by this processing notebook.

## Outputs

Intermediate exports document the processing stages. The complete final dataset is `outputs/Cookie_eye_tracking_with_survey_visits.sav`, with durations in seconds, short variable names, and total/mean AOI visit duration. `outputs/Cookie_eye_tracking_variable_names.csv` maps original names to short names and definitions.

## Sharing revisions

Clear all cell outputs and save a separate sharing copy before committing an executed notebook. Participant-level preview tables are generated during local execution. `.gitignore` excludes input/output folders when using Git, but browser uploads require selecting the intended code files yourself.
