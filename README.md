# Exoplanet Detection from Kepler KOI Data

The **Kepler Objects of Interest (KOIs)** dataset contains several candidate objects (flagged as such by the Kepler robovetter). A candidate can either be a planet or a flase alarm. Our goal is to use ML to detect Planets using **transit-derived physical features** (e.g., planet radius, stellar surface gravity, transit depth, transit duration) without using telescope-specific noise artifacts and classify them into:
- **Confirmed Planets**
- **False Positives**

---

## Dataset
The dataset is sourced from the [NASA Exoplanet Archive – Cumulative KOI Table](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=cumulative).

Data Cleaning and Preprocessing Steps:
- Transit features and telescopic error columns were removed.
- Objects labelled as 'Candidates' (may or may not be planets) were excluded.
- Missing rows (~3% of the dataset) were removed.
- Log-transformation of highly skewed features were done.
- Final training dataset included only **CONFIRMED** and **FALSE POSITIVE** KOIs

---

## Model Training

RandomForest, XGBoost and LightGBM models were trained and tested:

- Trained on ~7.5k labeled KOIs.
- Balanced class weights with Stratified 5-Fold Cross-Validation used to handle lopsidedness of the false-positive class.
- GridSearch used for Hyperparameter Tuning was optimised for F1 score.

**Best Model — LightGBM Classifier**

| Metric   | Value |
|----------|-------|
| Accuracy | ~0.89 |
| F1 Score | ~0.86 |
| ROC AUC  | ~0.99 |

---

## Observations

1. Feature importance analysis identified planetary radius, transit duration, and stellar surface gravity as the most influential features, consistent with physical expectations.

2. These features clearly separate true planets from impostors like eclipsing binaries and dwarf hosts from evolved giants. Together with transit duration and orbital period, they form a coherent geometric signature — encoding the precise, repeatable patterns expected of genuine planetary systems.

3. False positives are structurally noisy. They exhibit high variance and extreme values across multiple features, unlike confirmed planets which lie in tight, physically consistent ranges.

4. An AUC of ~0.99 confirms clear class separability. The Precision–Recall curve remained in a high-precision region across most recall values — the model retrieves many true planets before false positives begin to rise. The sharp drop only appears at very high recall where the threshold is pushed too low.

5. The confusion matrix showed a low number of missed real planets (3.3%), reflecting high recall while maintaining a controlled false positive rate.

---

## Outputs

1. Trained model that classifies KOIs.
2. CSV file containing a ranked list of candidate KOIs sorted by predicted planet probability.
