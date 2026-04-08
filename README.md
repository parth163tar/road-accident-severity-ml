# Road Accident Severity Prediction

A supervised machine learning project that predicts the severity of road accidents (Fatal, Serious, or Slight) using real UK accident data. Built for a car rental company use case: flagging high-risk trips before the keys are handed over.

---

## Business Problem

A car rental company wants to estimate how severe any accident a driver might be involved in is likely to be — not to refuse bookings, but to price risk more accurately, flag high-risk trips for safety briefings, and support smarter fleet management.

---

## Dataset

Two linked CSV files from a real UK road accident dataset:

| File | Description | Rows |
|------|-------------|------|
| `52_accidents.csv` | Accident circumstances (location, time, road/light conditions) | 5,001 |
| `52_vehicles.csv` | Vehicle and driver details | 7,822 |

Linked via `Accident_Index`. Merged dataset: **7,822 rows × 20 features**.

**Target variable:** `Accident_Severity` — Fatal (1), Serious (2), Slight (3)

> ⚠️ The dataset is highly imbalanced: ~82% Slight, ~16% Serious, ~1.5% Fatal.

---

## Project Structure

```
road-accident-severity-ml/
│
├── data/
│   ├── 52_accidents.csv        # Accident circumstances
│   └── 52_vehicles.csv         # Vehicle and driver data
│
├── notebook/
│   └── BBM110_final.ipynb      # Full analysis notebook
│
├── report/
│   └── Final_Draft.pdf         # Submitted PDF report
│
├── requirements.txt            # Python dependencies
├── .gitignore
└── README.md
```

---

## Methodology

### Preprocessing
- Merged two datasets on `Accident_Index`
- Replaced sentinel `-1` values with `NaN`
- Median imputation for numeric columns, mode for categorical
- IQR-based outlier detection (outliers retained — real-world values)
- Label encoding for categorical features
- Feature selection: Pearson correlation + SelectKBest (ANOVA F-statistic)
- Stratified 80/20 train/test split

### Models

| Model | Hyperparameters Tuned |
|-------|-----------------------|
| Decision Tree | `max_depth`, `min_samples_leaf`, `class_weight='balanced'` |
| Random Forest | `n_estimators`, `max_depth`, `min_samples_split`, `class_weight='balanced'` |

Both models use `class_weight='balanced'` to improve sensitivity to rare Serious and Fatal accidents.

A `DummyClassifier` (most-frequent strategy) serves as the baseline.

---

## Results

| Model | Accuracy | Macro F1 | Serious Recall |
|-------|----------|----------|----------------|
| Baseline (Dummy) | 0.822 | 0.30 | 0.00 |
| Decision Tree | 0.919 | **0.657** | **0.83** |
| Random Forest | 0.914 | 0.631 | 0.66 |

**5-fold cross-validation (accuracy):**
- Decision Tree: Mean = 0.929, Std = 0.008
- Random Forest: Mean = 0.918, Std = 0.005

For the car rental use case, **the Decision Tree is the better model** — it achieves higher Serious recall (0.83 vs 0.66), meaning it catches more of the high-risk trips that matter most. The Random Forest has better precision on Serious (0.80 vs 0.74) but misses more real cases.

---

## Key Findings

- **Speed limit** is the strongest predictor of severity — higher speed roads produce worse outcomes
- **Vehicle age** has the highest ANOVA F-score (598), indicating strong class separation
- **Young drivers** (under 25) appear in accidents more often than their road use share would predict
- **Unlit darkness** correlates with a disproportionate share of Serious and Fatal accidents
- **Time of day** matters — late-night and early-morning accidents tend to be more severe

---

## How to Run

**1. Clone the repository**
```bash
git clone https://github.com/YOUR_USERNAME/road-accident-severity-ml.git
cd road-accident-severity-ml
```

**2. Install dependencies**
```bash
pip install -r requirements.txt
```

**3. Add the data files**

Place `52_accidents.csv` and `52_vehicles.csv` in the `data/` folder.

**4. Run the notebook**
```bash
jupyter notebook notebook/BBM110_final.ipynb
```

> The notebook expects CSV files in the same directory when running. Either copy them next to the notebook or update the `pd.read_csv()` paths to `'../data/52_vehicles.csv'` etc.

---

## Technologies

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3+-orange)
![pandas](https://img.shields.io/badge/pandas-2.0+-green)
![Jupyter](https://img.shields.io/badge/Jupyter-notebook-yellow)

- **Python 3.10+**
- **pandas** — data manipulation
- **numpy** — numerical operations
- **matplotlib / seaborn** — visualisation
- **scikit-learn** — modelling (DecisionTreeClassifier, RandomForestClassifier, SelectKBest, DummyClassifier, cross_val_score)

---

## Future Improvements

- Apply **SMOTE** to address Fatal class imbalance
- Try **Gradient Boosting / XGBoost** for improved performance
- Add features: trip distance, weather, driver history, vehicle safety rating
- Build a simple **risk scoring API** for integration with a booking system

---

## Author

**Aston University — BBM110 Machine Learning for Business Analytics**  
Academic Year 2025/26

*Note: Generative AI (Claude) was used as a support tool to assist with writing Python code. All written commentary and analysis is my own.*
