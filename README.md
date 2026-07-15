# AInWater Time Series Anomaly Detection

A Python project for detecting anomalies, sensor outliers, and drift patterns in water-quality time series. The repository combines a reusable command-line pipeline with exploratory notebooks for preprocessing, drift analysis, model evaluation, and manual labeling workflows.

This is a cleaned public portfolio version of an academic/capstone project. Private user/company datasets and generated reports are intentionally excluded.

## Highlights

- Modular anomaly detection pipeline for multivariate time-series CSV files.
- Two detection strategies:
  - `diff`: centered-difference detection with iterative replacement of outliers.
  - `adaptive_variance`: residual-based detection using AR, MA, or ARMA models plus ChangeFinder for regime-change scoring.
- Per-variable JSON configuration for model and detector parameters.
- Batch processing for all configured variables or selected columns.
- Preprocessing and analysis notebooks for missing-data gaps, outliers, drift, and model experiments.
- Research utilities for automated data-quality analysis and drift reporting.

## Repository Structure

```text
.
├── outlier_pipeline/
│   ├── main.py                         # CLI entry point
│   ├── pipeline.py                     # DataPipeline orchestration
│   ├── outlier_detectors.py            # Diff and adaptive-variance detectors
│   ├── time_series_models.py           # AR, MA, and ARMA residual models
│   ├── generar_config.py               # Generate JSON config from a CSV
│   ├── demo_data.csv                   # Small demo dataset
│   ├── demo_config.json                # Demo configuration
│   └── config_parametros/              # Parameter-analysis helpers
├── research_project/
│   ├── Analisis/                       # Missing data, outlier, and drift analysis notebooks
│   ├── Drift Evaluation/               # Drift labeling and evaluation notebooks
│   ├── Modelos/                        # AR and differencing model experiments
│   ├── Pre-Procesamiento/              # Preprocessing notebooks and helpers
│   ├── pipeline/                       # Automated analysis pipeline modules
│   └── timeseries-labeler-main/        # Manual time-series labeling tool
├── DATA_NOTES.md                       # Public-data and privacy policy
└── requirements.txt
```

## Data Privacy

The original project used real water-quality monitoring data. That data is not included here.

Excluded from this public version:

- Raw plant/user time-series data.
- Processed plant CSV files and manual labels.
- Generated HTML reports and drift-report exports.
- Trained model artifacts such as `.pkl` files.
- Virtual environments, caches, and local Git metadata.

Notebook outputs were stripped before publication so charts or tables from private datasets are not embedded in the repository. To reproduce experiments, use anonymized or public sample data under a local `data/` folder.

## Installation

```bash
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt
```

On macOS/Linux:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Input Format

The main pipeline expects a CSV file with:

- A `date_time` column in a parseable datetime format.
- One or more numeric sensor/variable columns.
- Missing values are allowed and are skipped during per-variable processing.

Example:

```csv
date_time,temperature,pressure,flow
2025-01-01 00:00:00,25.3,101.2,150.5
2025-01-01 00:05:00,25.5,101.3,151.2
2025-01-01 00:10:00,25.4,101.1,150.8
```

## Quick Start

Run the included demo from the pipeline folder:

```bash
cd outlier_pipeline
python main.py demo_data.csv --config demo_config.json --output-dir demo_output --all
```

The output is one labeled CSV per processed variable:

```text
demo_output/
└── <column>_labeled.csv
```

For the `diff` detector, output columns include:

- `date_time`
- `value`
- `valores_sin_outliers`
- `label`

For the `adaptive_variance` detector, output columns include:

- `date_time`
- `value`
- `residual`
- `outlier_score`
- `change_score`
- `label`

## Generate a Configuration File

Use `generar_config.py` to create a starter configuration from any compatible CSV:

```bash
cd outlier_pipeline
python generar_config.py path/to/data.csv --output config.json
```

Then run the pipeline:

```bash
python main.py path/to/data.csv --config config.json --output-dir output --all
```

Process only selected variables:

```bash
python main.py path/to/data.csv --config config.json --columns temperature pressure
```

## Configuration Example

```json
{
  "temperature": {
    "ts_model": "MA",
    "ts_params": {
      "q": 2,
      "alpha": 0.005,
      "quantile": 0.995,
      "factor_olvido": 0.02,
      "lag_cambio": 2,
      "suavizado": 7,
      "change_quantile": 0.99
    },
    "outlier_detector": "diff",
    "outlier_params": {
      "lambda_centrada": 12,
      "k": 0
    }
  }
}
```

## Methods

### Centered-Difference Detector

The `diff` detector compares each point with neighboring values, computes a centered-difference score, and flags abrupt local deviations. It can also replace detected outliers iteratively with smoothed estimates, producing a cleaned series in `valores_sin_outliers`.

### Adaptive-Variance Detector

The `adaptive_variance` detector first fits a time-series model (`AR`, `MA`, or `ARMA`) and analyzes residuals. It then computes an adaptive variance score for outliers and uses ChangeFinder to score possible regime changes. Labels can be `normal`, `outlier`, or `change`.

## Tech Stack

- Python
- pandas and NumPy
- statsmodels
- changefinder
- scikit-learn
- matplotlib / Plotly / Dash for analysis and labeling workflows
- Jupyter notebooks for experiments and validation

## Project Status

This repository is prepared as a public portfolio artifact. The algorithmic code, demo data, configuration examples, and notebooks are included, while private operational data is excluded by design.
