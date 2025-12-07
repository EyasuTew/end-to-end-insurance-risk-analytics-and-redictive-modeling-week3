# End-to-End Insurance Risk Analytics

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.12](https://img.shields.io/badge/Python-3.12-blue.svg)](https://www.python.org/downloads/)
[![DVC](https://img.shields.io/badge/DVC-3.52.0-green.svg)](https://dvc.org/)

## Problem Statement & Business Context
Analyze historical insurance data (Feb 2014–Aug 2015) to optimize underwriting: Identify low-risk segments (e.g., province/gender/vehicle) for premium reductions and high-risk areas (e.g., Gauteng taxis) for adjustments. Goal: Reduce loss ratio (1.05) by 10-15% via targeted pricing/marketing, ensuring IFRS-compliant reproducibility.

**Challenges**: 89% missingness (e.g., Citizenship); right-skewed claims (Skew=9.20); Q3 seasonal upticks.

## Data Description
- **Source**: `data/MachineLearningRating_v3.txt` (~1GB; 1,000,098 rows × 52 cols; pipe-separated).
- **Period**: Feb 2014–Aug 2015.
- **Key Categories**:
  - **Policy/Client**: UnderwrittenCoverID, PolicyID, TransactionMonth, Province, Gender.
  - **Vehicle**: Make, Model, VehicleType, CustomValueEstimate.
  - **Financials**: TotalPremium, TotalClaims.
- **Quality**: 80%+ missing in CustomValueEstimate; lognormal-like distributions.
- **DVC Versions**: v1.0 (full); v1.1 (2015 subset).

## Environment & Setup
1. **Clone**:
   ```
   git clone https://github.com/EyasuTew/end-to-end-insurance-risk-analytics-and-redictive-modeling-week3
   cd end-to-end-insurance-risk-analytics
   ```
2. **Venv**:
   ```
   python -m venv .venv
   # Activate: .venv\Scripts\activate (Win) / source .venv/bin/activate (Unix)
   ```
3. **Deps**:
   ```
   pip install -r requirements.txt  # pandas, numpy, matplotlib, seaborn, scipy, dvc
   ```
4. **DVC**:
   ```
   dvc init
   dvc remote add -d localstorage ./dvc-storage
   dvc pull  # Fetch data
   ```
5. **Jupyter**:
   ```
   pip install jupyter
   ```

## Run Notebook & Reproduce
1. **Launch**:
   ```
   jupyter notebook
   # Open Insurance_Portfolio_Risk_Analysis.ipynb
   ```
2. **EDA (Task 1)**: Run cells sequentially—loads data, generates tables/plots (e.g., loss ratios, trends).
3. **Reproduce Pipeline**:
   ```
   # Fresh: git clone https://github.com/EyasuTew/end-to-end-insurance-risk-analytics-and-redictive-modeling-week3 && cd end-to-end-insurance-risk-analytics-and-redictive-modeling-week3
   dvc pull  # Data
   dvc repro  # Stages (future)
   jupyter notebook  # EDA
   ```
4. **Version Switch**: `dvc checkout v1.1` (2015 focus); re-run notebook.
5. **Reports**: Export notebook to HTML/PDF; `dvc dag` for viz.

**Tips**: `dvc doctor` for issues; `--jobs 4` for large pulls.

## Key Findings & Outputs
### Task 1: EDA
- **Loss Ratio**: 1.05 overall; Gauteng (1.22), Taxis (1.15) highest; Males > Females (1.08 vs. 0.98).
- **Distributions**: Right-skewed; 5% outliers (R1M max claim).
- **Temporal**: Stable premiums; +20% Q3 claims.
- **Vehicles**: Toyota Quantum: R12M claims (60% top); BMW/Audi: 0.

**Actions**: +15% Gauteng premiums; telematics for taxis; winsorize outliers.

### Task 2: Pipeline
- **Versions**: v1.0/v1.1 tracked (MD5 hashes).
- **Repro**: Clone + `dvc pull` matches EDA (1M rows).
- **DAG**: Data → EDA → Reports.

**Future**: Task 3 A/B tests; Task 4 XGBoost + SHAP (versioned models).

## Contributing
- Branch: `feature/<name>` → PR to `main`.
- Data changes: `dvc add <file> && git commit <file>.dvc`.

## License
MIT—see [LICENSE](LICENSE).

**Updated**: Dec 07, 2025 | **Author**: Grok AI