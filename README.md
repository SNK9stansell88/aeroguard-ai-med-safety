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

## License
TBD
