# Cookie eye-tracking data processing

JupyterLab code for processing Tobii Pro Lab exports from a Single versus Multiple cookie-display study and joining the resulting measures to survey responses.

## Repository contents

- `cookie_eye_tracking_analysis.ipynb`: the data-processing notebook. Saved cell outputs and execution counts have been cleared from this sharing copy.
- `requirements.txt`: package versions installed in the current local environment when this sharing copy was prepared.

Participant data and generated datasets are not included.

## What the notebook does

The notebook aggregates fixation metrics across five AOI positions, imports interval and visit metrics, constructs saccade categories, merges survey responses using Recording = ExperimentID, derives ratios and revisit measures, and exports SPSS datasets with variable definitions. Duration measures are converted to seconds, and the final variable names use at most eight characters.

The Single condition uses the same five AOI positions as the Multiple condition. The final cells also calculate combined AOI visit duration and average duration per AOI visit.

## Running locally

1. Download the repository files into a local folder.
2. Activate a Python environment with JupyterLab. The notebook metadata records Python 3.14.4.
3. Install the listed dependencies in that environment:

   ```bash
   python -m pip install -r requirements.txt
   ```

4. Supply the necessary Tobii exports and survey input locally. The exports are organized into `Group 1_CookieSingle` and `Group 2_CookieMultiple`; fixation and visit exports are in each group's `TotalFix_Visit` subfolder.
5. Update the notebook's `DATA_DIR`, `survey_path`, `study_dir`, and `output_path` settings for your computer. The analysis code is preserved from the original notebook, including its original local paths. These paths need editing before another computer can run the notebook.
6. Launch JupyterLab with `jupyter lab`, open the notebook, and run its cells in order.

The sharing copy was checked to preserve every original code cell and to contain no saved outputs. The complete data-processing pipeline was not rerun as part of preparing this copy.

## Updating the notebook on GitHub

Work on your local analysis notebook. Prepare a separate copy for sharing, clear its cell outputs, save it, and upload the updated copy. Notebook outputs can contain participant-level records even when the source data files are not uploaded.

## Publication

Add the article citation and a description of how the research data can be accessed when those details are available. Choose a software license when ready to specify how others may reuse the code.
