# Early Detection of GI Cancers Using Blood Biomarkers and Machine Learning

Can a simple blood test help detect gastric and colorectal cancers early?
This project uses **39 blood protein biomarkers** plus age, sex, and race from the CancerSEEK study to classify **GI cancer vs. normal** samples with machine learning.

**Course:** INFO-B 501, Indiana University Indianapolis (team project, Spring 2025)
**Team:** Frederick Onyango, Geethanjali Karuturi, Mahitha Gogu, Rashmita Kudamala
**My role (Geethanjali):** exploratory data analysis, clinical threshold engineering (flagging abnormal biomarker levels using published cutoffs), and interpreting biomarker distributions.

---

## Key Results

| Model | Sampling method | AUC | Recall | F1 |
|---|---|---|---|---|
| Random Forest | Undersampling | **0.94** | **0.86** | **0.82** |
| Random Forest | SMOTE | 0.92 | 0.83 | 0.79 |
| Logistic Regression | Undersampling | 0.91 | 0.81 | 0.78 |
| Logistic Regression | SMOTE | 0.89 | 0.78 | 0.76 |

- Final dataset: **1,255 samples** (812 normal, 453 GI cancer)
- Top predictive biomarkers (Random Forest): IL-6, Prolactin, TIMP-2, PAR, CEA, CD44, sEGFR, sPECAM-1
- Recall was prioritized because missing a cancer case (false negative) is the most costly error in screening.

---

## Workflow

```
CancerSEEK data (Cohen et al., 2018, Science) → MySQL database
        ↓
Merge biomarker + demographic tables on Sample ID
        ↓
Clean: remove symbols (*, ^, commas), convert to numbers,
keep only Colorectal, Stomach, Normal samples, drop rows with missing values
        ↓
Clinical threshold flags (e.g., CEA > 5,000 pg/mL = elevated)
        ↓
Exploratory analysis: distributions, outliers, correlations, demographics
        ↓
Statistical tests: t-test, chi-square, Mann-Whitney U
        ↓
Handle class imbalance: SMOTE, oversampling, undersampling, bootstrapping
        ↓
Random Forest + Logistic Regression (80/20 stratified split)
        ↓
Evaluate: AUC, precision, recall, F1, confusion matrices, ROC curves
```

---

## Repository Structure

```
gi-cancer-biomarker-classification/
├── notebooks/
│   └── gi_cancer_classification.ipynb   # full analysis with outputs
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Data

The data comes from the supplementary tables (Tables S4 and S6) of:
Cohen, J. D. et al. (2018). *Detection and localization of surgically resectable cancers with a multi-analyte blood test.* Science, 359(6378), 926–930. https://doi.org/10.1126/science.aar3247

The data is **not stored** in this repo. In the original project, the tables were loaded into a course MySQL database and read into Python with `MySQLdb`. To rerun the notebook, load the two tables into your own MySQL database, or replace the first loading cells with `pd.read_csv()` on the downloaded tables.

## Tools

Python · pandas · NumPy · SciPy · scikit-learn · imbalanced-learn · Matplotlib · Seaborn · MySQL

## Limitations

- Several biomarkers have no universal clinical cutoff, so some thresholds came from literature and unit conversions.
- SMOTE creates synthetic samples, which may not reflect real patients.
- The dataset is fairly small, so results need validation in larger, independent cohorts.
