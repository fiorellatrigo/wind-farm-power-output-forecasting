# Wind Farm Power Output Forecasting & Efficiency Optimization

## 1. The Problem
Wind energy generation is highly intermittent. Failing to predict exact power output leads to severe financial grid penalties for non-compliance and causes the structural wasting of clean, unutilized energy. This volatility directly impacts energy traders committing power volumes to the market and Transmission System Operators (TSOs) managing real-time grid stability.

## 2. Objective
The primary objective of this project is to develop an optimized predictive framework to forecast regional wind power generation (`wind_generation_mw`), minimizing average predictive errors against baseline models. This allows TSOs and energy traders to make accurate, evidence-based asset storage and reserve-capacity allocation decisions.

## 3. Methodology
### Data & Target
* **Source:** Processed historical 15-minute interval records from four major German TSOs (50Hertz, Amprion, TenneTTSO, and TransnetBW) tracking `wind_generation_mw`.
* **Volume:** Training Set (121,958 rows) and Testing Set (30,490 rows).

### Structural Restructuring (Melt Pipeline)
The dataset was originally structured in a "wide format" where each row represented a single calendar day containing 96 independent 15-minute operational measurement columns. We implemented a data transformation pipeline that flattened the daily metrics into a continuous time-series "long format," mapping a unique `timestamp` index to each power generation observation.

### Feature Engineering & Validation Strategy
* **Features:** Extracted cyclical temporal features (hour, day, month, day of week) to model seasonal operational signatures.
* **Validation Split:** To avoid target leakage and respect time dependencies, data was strictly partitioned chronologically using an 80/20 train-test split instead of randomized cross-validation.
* **Modeling:** Trained and hyperparameter-tuned an **XGBoost Regressor** framework to capture non-linear regional operational patterns against time constraints.

## 4. Findings
### Model Performance vs. Baseline

| Model Architecture | MAE (MW) | RMSE (MW) | R² Score |
| --- | --- | --- | --- |
| **Naive Constant Baseline** | 86.96 | 97.88 | -0.9164 |
| **XGBoost Regressor** | **49.41** | **77.15** | **-0.1905** |

* **Error Reduction:** The optimized XGBoost framework reduced the average predictive error by over **37 MW** compared to a naive historical mean baseline.
* **The Atmospheric Boundary:** While the gradient-boosting trees successfully isolated geographical distribution baselines and quarterly seasonal trends (beating raw guessing), the negative $R^2$ score under strict chronological testing mathematically proves that calendar attributes alone introduce structural noise rather than physical predictive signals.
* **Geographical Dominance:** Coastal-adjacent TSOs (TenneTTSO and 50Hertz) contain the highest structural generation capacities and operational variance, while landlocked grids stay tightly compressed.
* **The Calendar Ceiling:** Temporal boundaries successfully isolate large seasonal capacity limits (such as winter production peaks), but isolated calendar variables act as structural noise for precise hourly predictions.

## 5. Recommendations
* **Contextual Trading:** Utilize the 37 MW optimization threshold to adjust spot-market trading risk parameters during high-variance seasonal transitions.
* **Spatial-Temporal Enrichment:** Perform data enrichment by joining unified grid timestamps with historical physical weather APIs (such as Open-Meteo) to inject **wind speed (m/s)** and air density features. This physics-dependency injection is critical since time-series architectures alone cannot fully capture sudden meteorological spikes governed by fluid dynamics.

## 6. Technologies
* **Language:** Python 3.9
* **Data Manipulation & Pipelines:** Pandas, NumPy
* **Machine Learning Framework:** Scikit-Learn, XGBoost
* **Visualization & Analytics:** Matplotlib, Seaborn

---

## Project Structure
```text
├── datasets/              # Local directory for raw datasets
├── img/                   # Generated data visualizations and charts 
├── notebooks/             # Jupyter Notebooks containing EDA and Modeling
├── README.md              # Project executive summary and documentation
└── requirements.txt       # Project dependencies

```

---

## Installation & Environment Setup

This project uses an isolated Python environment. To replicate this setup, run the following commands in your terminal:

### 1. Clone the repository

```bash
git clone [https://github.com/fiorellatrigo/wind_energy_prediction](https://github.com/fiorellatrigo/wind-farm-power-output-forecasting)
cd wind-farm-power-output-forecasting

```

### 2. Create and activate the virtual environment

* **Windows:**

```powershell
python -m venv env
.\env\Scripts\activate

```

* **Mac/Linux:**

```bash
python -m venv env
source env/bin/activate

```

### 3. Install dependencies

```bash
pip install -r requirements.txt

```

---

## References & Data Sources

* **[German TSO Open Data](https://www.kaggle.com/datasets/jorgesandoval/wind-power-generation):** 50Hertz, Amprion, TenneTTSO, and TransnetBW grid management reports.
* **Target Variable:** Actual wind power generation measured in Megawatts (MW).
