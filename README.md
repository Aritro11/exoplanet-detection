# Exoplanet Detection from Kepler KOI Data

The **Kepler Objects of Interest (KOIs)** dataset contains several candidate objects (flagged as such by the Kepler robovetter). A candidate can either be a planet or a flase alarm. Our goal is to use ML to detect Planets using **transit-derived physical features** (e.g., planet radius, stellar surface gravity, transit depth, transit duration) without using telescope-specific noise artifacts and classify them into:
- **Confirmed Planets**
- **False Positives**

---

## Dataset
The dataset is sourced from the [NASA Exoplanet Archive – Cumulative KOI Table](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=cumulative).

After data-preprocessing, cleaning:
- Objects labelled as 'Candidates' (may or may not be planets) were excluded
- Missing rows were removed
- Final training dataset included only **CONFIRMED** and **FALSE POSITIVE** KOIs

---

## Model

The final model used is:

**LightGBM Classifier**
- Balanced class weights to handle imbalance
- Tuned using cross-validation and F1 score optimization
- Trained on ~7.5k labeled KOIs

Performance (Test Set):
| Metric | Value |
|------|------|
| Accuracy | ~0.89 |
| F1 Score | ~0.86 |
| ROC AUC | ~0.99 |

---

## Outputs
1. **Trained model** that classifies KOIs.
2. CSV file having **Ranked list of candidate KOIs** sorted by predicted planet probability.
