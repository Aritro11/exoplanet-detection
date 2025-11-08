# 🪐 Exoplanet Detection from Kepler KOI Data

This project builds a machine learning model to classify **Kepler Objects of Interest (KOIs)** into:
- **Confirmed Planets**
- **False Positives**

The classification is based on **transit-derived physical features** (e.g., planet radius, stellar surface gravity, transit depth, transit duration) without using telescope-specific noise artifacts.

---

## 📂 Dataset
The project uses the **Kepler KOI cumulative catalog** from NASA Exoplanet Archive.

After cleaning:
- Candidates were excluded (no confirmed label)
- Missing rows were removed
- Final training dataset included only **CONFIRMED** and **FALSE POSITIVE** KOIs

---

## ⚙️ Model
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

## ⭐ Outputs
1. **Trained model** that classifies KOIs
2. **Ranked list of candidate KOIs** sorted by predicted planet probability
