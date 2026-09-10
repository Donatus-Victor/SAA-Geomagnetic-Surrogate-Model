# SAA Geomagnetic Surrogate Model

A machine-learning surrogate model that reconstructs Earth's magnetic field over the South Atlantic and automatically locates the **South Atlantic Anomaly (SAA)** — the region where the field is unusually weak.

Built on IGRF (International Geomagnetic Reference Field) data, this project mirrors a real-world geophysical exploration workflow: turn scattered measurements into a smooth, continuous map, then use that map to find anomalies and structural boundaries.

## What it does

1. **Cleans messy source data** — converts a human-readable spreadsheet layout into a tidy table (one row per location, one column per measurement).
2. **Validates the data** — diagnostic plots check that values are physically plausible before modeling.
3. **Removes bad readings** — filters out entries outside physically possible bounds for Earth's magnetic field.
4. **Trains a surrogate model** — fits a **Gaussian Process Regressor** (equivalent to Kriging) to interpolate the magnetic field continuously across the region, benchmarked against a **Random Forest Regressor** baseline.
5. **Locates the anomaly automatically** — queries the trained model to find the weakest point in the field, with no manual input, and validates it against the known real-world SAA location.
6. **Separates trend from residual** — decomposes the field into a broad regional trend and local residual anomalies, since local variation is what matters for subsurface interpretation.
7. **Detects gradients/edges** — computes spatial gradients to highlight sharp transitions that can indicate geological boundaries (e.g., faults, rock-type changes).

## Why it matters

- Mirrors an early-stage **oil & gas / mineral exploration** workflow, where magnetic surveys are among the cheapest tools used before drilling or seismic surveys.
- Provides a **repeatable, automated pipeline**: raw data → clean data → surrogate model → anomaly detection → structural map.
- **Self-validating**: the model finds the real SAA location without being told where to look, confirming it has learned real physical structure rather than memorizing data.
- The pipeline pattern (surrogate model → trend/residual separation → edge detection) generalizes to other spatial-prediction and exploration problems.

## Tech stack

- **Python**, **Jupyter Notebook**
- `pandas`, `numpy` — data wrangling
- `matplotlib` — diagnostic and anomaly visualizations
- `scikit-learn`:
  - `GaussianProcessRegressor` + kernels (Kriging-style surrogate model)
  - `RandomForestRegressor` (baseline comparison)
  - `cross_val_predict`, `r2_score` (model validation)

## Getting started

### Prerequisites
```bash
pip install pandas numpy matplotlib scikit-learn jupyter
```

### Run
```bash
jupyter notebook SAA_Surrogate_Model_documented.ipynb
```

Run all cells top to bottom — later cells depend on the cleaned dataset and trained model from earlier steps.

## Project structure

```
.
├── SAA_Surrogate_Model_documented.ipynb   # Main notebook: data cleaning, modeling, analysis
├── README.md
└── (data files, as applicable)
```

## Results summary

- The Gaussian Process surrogate model outperforms the Random Forest baseline for interpolating the magnetic field.
- The model-predicted anomaly location closely matches the known location of the real South Atlantic Anomaly.
- Trend/residual decomposition and gradient maps highlight local structure suitable for further geological interpretation.
