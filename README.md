# AeroGuard-AI — System-Level Medication Safety (EMS / Air Medical)

[![GitHub Release](https://img.shields.io/github/v/release/SNK9stansell88/aeroguard-ai-med-safety?color=blue&label=Release)](https://github.com/SNK9stansell88/aeroguard-ai-med-safety/releases)
[![Made with Python](https://img.shields.io/badge/Made%20with-Python-blue.svg)]()
[![Google Colab](https://img.shields.io/badge/Open%20in-Colab-orange.svg)]()

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/SNK9stansell88/aeroguard-ai-med-safety/blob/main/notebooks/01_data_forecast_baseline.ipynb)



# AeroGuard-AI — System-Level Medication Safety (EMS / Air Medical)

**Goal:** Predict *where, when, and how* medication harm will occur across the system and **optimize** limited safety actions (not just report audits/training). This project complements, not replaces, bedside tools like Handtevy.

## What’s in this repo
- `notebooks/` — reproducible Colab notebooks for data prep, forecasting, and modeling  
- `src/` — production-style code (ETL, forecasting, ML, uplift, RL/optimization, monitoring)  
- `reports/figures/` — forecast plots, calibration/ROC/PR, dashboards  
- `models/` — saved models/policies (artifacts)  
- `data/` — **local only** (de-identified); not committed

## End-to-end pipeline (executive view)
1. **Data engineering**: ePCR/QA + ops context → clean, de-ident, feature tables (DuckDB/Python).  
2. **Forecasting**: hierarchical forecasts of med-error *counts/rates* by base/med/segment.  
3. **Calibrated harm model**: event-level risk (GBDT + NLP) with Brier/Calibration checks.  
4. **Causal uplift**: which safeguards (second-check, pump guardrails, micro-drills) actually reduce harm for whom.  
5. **Optimization & RL**: allocate limited interventions to maximize expected harm avoided under budget/fairness/coverage constraints.  
6. **Monitoring**: drift, calibration, SPC, bias audits; model card & governance.

## Quick start (Colab)
- Open the notebook in `notebooks/00_colab_quickstart.ipynb` (to be added).  
- Upload your de-identified CSV, run all cells to generate:
  - forecasts, harm model, top-risk medication table  
  - metrics (AUROC/AUPRC/Brier) and calibration plots  
  - saved artifacts in `models/`

## Non-goals
- Not a bedside dosing calculator (Handtevy already covers that).  
- This is enterprise risk prediction & resource optimization.

## Safety & ethics
- Human-in-the-loop safety  
- Model Card & bias checks (in `docs/`, to be added)  
- Alert-fatigue budgets and rollback plan
## Results (Baseline Models)

### 1. Medication-Error Forecasting (ETS)
- Model: Exponential Smoothing (additive trend + seasonality)
- Forecast horizon: 6 months
- Output: Monthly event counts with 95% prediction intervals
- Artifacts:
  - `reports/figures/forecast_events_ets.png`
  - `reports/monthly_counts.csv`

**Executive interpretation:**  
The forecast shows a rising seasonal pattern in medication-error events. If this trend continues, expected increases should impact QA workload, drug restocking, training, and protocol reviews.

---

### 2. Harm-Risk Model (GBDT + Probability Calibration)
- Model: Gradient Boosting Classifier
- Calibrated with: `CalibratedClassifierCV(method="sigmoid")`
- Evaluation (test set):
  - **ROC AUC:** ~0.93  
  - **PR AUC:** ~0.87  
  - **Brier Score:** ~0.05  
- Artifacts:
  - `artifacts/harm_model_calibrated.joblib`
  - `artifacts/metrics_harm.json`
  - `reports/figures/harm_model_roc_pr.png`
  - `reports/figures/harm_model_calibration.png`
  - `reports/top_meds_predicted_risk_last90.csv`

**Executive interpretation:**  
The calibrated model produces reliable probability estimates of clinically meaningful harm.  
This allows targeting high-risk medications, bases, or clinical patterns **before** adverse events occur.

---

### What this enables
✅ Forecast expected medication-error workload  
✅ Identify bases/medications at highest predicted risk  
✅ Calibrated probabilities = real clinical meaning  
✅ Artifacts are reproducible and production-friendly (joblib + CSV)  

This baseline establishes the foundation for:
- RL optimization of safety interventions
- Model monitoring / drift detection
- Deployment into a dashboard or API

- ---

## 🔁 How to Reproduce

You can run the full pipeline (data cleaning → forecasting → harm model → calibration plots → saved artifacts) in Google Colab.

**1. Open the notebook**
- `notebooks/01_data_forecast_baseline.ipynb`
- Click: **"Open in Colab"** or use this link:  
  https://colab.research.google.com/github/SNK9stansell88/aeroguard-ai-med-safety/blob/main/notebooks/01_data_forecast_baseline.ipynb

**2. Upload your local CSV**
The notebook will clean it, engineer features, run forecasting, and train a calibrated harm model.

**3. Outputs generated automatically**
- Forecasts
- PR / Calibration curves
- Top-risk medication table (last 90 days)
- Harm model metrics (AUROC/AUPRC/Brier)
- Trained model artifact (`harm_model_calibrated.joblib`)
- Project ZIP with everything: `medsafe_project.zip`

All saved to the repo structure defined in `/reports`, `/models`, `/data/processed`.

---

✅ Fully reproducible  
✅ No hidden local dependencies  
✅ Anyone can re-run with a single notebook

---

## 🚀 Roadmap (what’s coming)

✅ v0.1.0 — Baseline AI model  
• Forecast monthly med-error counts  
• Harm-risk prediction (XGBoost + NLP outcome text)  
• Plots, calibration, saved model artifacts  
• Colab quick-start with user's own CSV

🛠 v0.2.0 — Real-time scoring API  
• FastAPI model serving  
• “Top-risk medications in last 7 days” dashboard  
• Risk alerts to QA / Safety managers  
• Local Docker deployment / HIPAA-safe

🔬 v0.3.0 — Reinforcement Learning  
• Optimize where to place safety actions (training, checklists, pump guards)  
• Budget-constrained harm reduction


## License
TBD
