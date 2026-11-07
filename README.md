# Auto Dealer Experiment & Causal Analysis

An end-to-end Python notebook that designs an A/B test and measures the true
impact of a business change at a fictional Noida multi-brand car service chain
(18 dealerships). The project asks whether a change that appeared to improve
results was real or just seasonal.

## Summary of findings

- **Recommendation: EXTEND** (not a full rollout yet).
- Treated dealerships had a lower warranty claim rate (8.04% vs 9.50%),
  fewer 30-day repeat visits (11.06% vs 13.99%) and higher revenue per
  vehicle (+₹374, about +4.7%).
- However, treatment and control were assigned **by dealership** (D01–D09
  treated, D10–D18 control), so the real comparison is 9 vs 9 dealerships,
  not 45,000 vehicles. Row-level confidence intervals are too narrow and the
  effect is confounded with dealership differences.
- The historical difference-in-differences model was mis-specified (group
  indicator mapped to treated × post, parallel trends untested), so it is
  not relied on.

## What the notebook does

1. Configures file paths and column mappings
2. Loads and profiles sales, job cards, warranty claims, experiment and
   historical regional data
3. Cleans data: part-code supplier switch, missing failure codes, backwards
   odometer readings, duplicate checks
4. Designs the A/B test: hypotheses, primary / secondary / guardrail metrics,
   sample size and duration via power analysis
5. Checks sample-ratio mismatch and randomisation balance
6. Estimates treatment effects with confidence intervals (z-test, Welch
   t-test, bootstrap)
7. Runs historical difference-in-differences with pre-trend diagnostics and
   an event study
8. Segments effects and applies CUPED / regression adjustment
9. Exports result tables and a decision memo draft

## Data

The datasets are not included in this repository. Expected files:
`noida_vehicle_sales.csv`, `service_job_cards.csv`, `warranty_claims.csv`,
`experiment_dataset.csv`, `historical_region_panel.csv` and an optional
`part_code_mapping.csv`. Edit the `FILES`, `COLS`, `EXP` and `HIST`
dictionaries in Section 1 of the notebook to match your column names.

## How to run

1. Open the notebook in Google Colab or Jupyter.
2. Place the data files next to the notebook and update the mappings.
3. Run all cells. Outputs are written to `ab_test_outputs/`.

Requirements: Python 3, `pandas`, `numpy`, `scipy`, `statsmodels`,
`matplotlib`, `seaborn`. Random seed is fixed at 42.

## Key methodological notes

- The unit of analysis must match the unit of randomisation. For dealership
  assignment, use dealership-level inference (for example an exact
  permutation test over the 48,620 possible 9-of-18 assignments).
- CUPED variance reduction applied to revenue only; it does not reduce the
  required sample for the binary claim outcome.
- Part codes are mapped only through a verified crosswalk, and missing
  failure codes are kept as missing rather than imputed.

## Repository contents

- `car_service_ab_testing_causal_impact (1).ipynb`: analysis notebook
- `Experiment_Design_Document.pdf`: experiment design document
- `One-Page Decision Memo Enhanced Service Quality Checks Pilot.pdf`: one-page decision memo

## Limitations

Results come from a fictional dataset. Several data-linkage and
column-mapping issues are documented in the project report and should be
fixed before relying on the raw-table pipeline.
