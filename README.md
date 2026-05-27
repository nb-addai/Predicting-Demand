# 🛒 Retail Demand Forecasting & Pricing Optimization

> **End-to-end data analytics project** — from raw sales data to business-ready forecasts and a live Tableau dashboard.  
> Built with Python · SQL · Prophet · scikit-learn · Tableau

---

## 📌 Project Overview

Retail businesses lose billions annually to poor demand forecasting — either overstock (dead inventory) or stockouts (lost sales). This project builds a production-style forecasting pipeline on the **Rossmann Store Sales** dataset (1,115 stores, ~2.5 years of daily transactions) and quantifies the **economic value** of forecast accuracy improvement.

### Key Questions Answered
- Which stores, seasons, and promotions drive the most revenue?
- How accurately can we predict daily sales 4–6 weeks out?
- What is the dollar value of a 10% improvement in forecast accuracy?
- How do macroeconomic signals (CPI, consumer sentiment) affect store-level demand?

---

## 🏗️ Architecture

```
data/
├── raw/                    # Original downloads (gitignored)
│   └── rossmann/           # train.csv, store.csv, test.csv
├── processed/              # Feature-engineered CSVs
│   ├── train_features.csv
│   ├── test_clean.csv
│   └── macro_indicators.csv
└── retail_forecast.db      # SQLite database (or PostgreSQL)

scripts/
├── 01_ingest_data.py       # Download, engineer features, load to DB
├── 02_eda.py               # Exploratory analysis & visualisations
├── 03_models.py            # Baseline, SARIMA, Prophet models
├── 04_evaluate.py          # RMSE / MAPE / business impact analysis
└── 05_export_tableau.py    # Export views for Tableau

sql/
├── schema.sql              # Table definitions, views, indexes
└── queries/                # Ad-hoc analysis queries

notebooks/
├── 01_EDA.ipynb
├── 02_Modeling.ipynb
└── 03_Business_Impact.ipynb

outputs/
├── forecasts/              # Model prediction CSVs
├── charts/                 # Static PNGs for README
└── tableau/                # Tableau-ready exports
```

---

## 📊 Dataset

| Source | Description | Rows | Granularity |
|--------|-------------|------|-------------|
| [Rossmann Store Sales (Kaggle)](https://www.kaggle.com/c/rossmann-store-sales) | Daily store sales, promotions, holidays | ~1M | Store × Day |
| [FRED – CPIAUCSL](https://fred.stlouisfed.org/series/CPIAUCSL) | Consumer Price Index | Monthly | National |
| [FRED – UMCSENT](https://fred.stlouisfed.org/series/UMCSENT) | U Michigan Consumer Sentiment | Monthly | National |
| [FRED – RSXFS](https://fred.stlouisfed.org/series/RSXFS) | Advance Retail Sales ex Food | Monthly | National |

---

## 🤖 Models

| Model | Description | Avg MAPE (target) |
|-------|-------------|-------------------|
| Seasonal Naive | Prior-year same week as forecast | ~18–22% |
| SARIMA | Seasonal ARIMA with auto-order selection | ~13–16% |
| **Prophet + Regressors** | Facebook Prophet + promo & macro features | **~10–13%** |

---

## 💰 Business Impact

A reduction in MAPE from **20% → 12%** across 1,115 stores translates to:

- **Stockout reduction**: ~4–6% fewer lost-sale events
- **Overstock reduction**: ~8–12% reduction in clearance markdowns
- **Estimated annual value**: $2M–$5M for a mid-size retailer (modelled in `notebooks/03_Business_Impact.ipynb`)

---

## 🚀 Getting Started

### 1. Clone & install

```bash
git clone https://github.com/YOUR_USERNAME/retail-demand-forecasting.git
cd retail-demand-forecasting
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
```

### 2. Configure credentials

```bash
cp .env.example .env
# Edit .env with your Kaggle and FRED API keys
```

Place `kaggle.json` at `~/.kaggle/kaggle.json` (download from [kaggle.com/settings](https://www.kaggle.com/settings)).

### 3. Run the pipeline

```bash
# Step 1 – Download data, engineer features, load to DB
python scripts/01_ingest_data.py

# Step 2 – Exploratory analysis
python scripts/02_eda.py

# Step 3 – Train & evaluate models
python scripts/03_models.py

# Step 4 – Business impact analysis
python scripts/04_evaluate.py

# Step 5 – Export for Tableau
python scripts/05_export_tableau.py
```

Or run end-to-end:
```bash
for script in scripts/0*.py; do python "$script"; done
```

### 4. Explore in notebooks

```bash
jupyter lab
```

Open `notebooks/01_EDA.ipynb` to begin interactive exploration.

---

## 📈 Tableau Dashboard

🔗 **[View live on Tableau Public](#)** *(link added after publish)*

The dashboard includes:
- **Store Performance Overview** — actual vs. forecast by store and region
- **Forecast Accuracy Tracker** — model MAPE comparison over rolling periods
- **Promo Impact Analysis** — revenue uplift from promotions
- **Scenario Planner** — adjust promo spend and see projected revenue impact

---

## 🗂️ SQL Schema Highlights

```sql
-- Actual vs forecast with error metrics
SELECT * FROM v_actual_vs_forecast LIMIT 100;

-- Model accuracy leaderboard
SELECT * FROM v_model_leaderboard;

-- Monthly store performance
SELECT * FROM v_monthly_store_performance
WHERE store_type = 'a' AND year = 2014
ORDER BY total_sales DESC;
```

Full schema: [`sql/schema.sql`](sql/schema.sql)

---

## 🛠️ Tech Stack

| Layer | Tools |
|-------|-------|
| Data ingestion | Python, Kaggle API, FRED API |
| Storage | SQLite / PostgreSQL, SQLAlchemy |
| Feature engineering | pandas, numpy |
| Statistical models | statsmodels (SARIMA), pmdarima |
| ML forecasting | Facebook Prophet |
| Evaluation | scikit-learn metrics |
| Visualisation | matplotlib, seaborn, plotly |
| BI dashboard | Tableau Public |
| Financial modelling | Excel (see `outputs/business_impact_model.xlsx`) |

---

## 📁 Key Files

| File | Purpose |
|------|---------|
| `scripts/01_ingest_data.py` | Data ingestion + feature engineering |
| `sql/schema.sql` | Full DB schema with views |
| `notebooks/01_EDA.ipynb` | Exploratory analysis |
| `notebooks/03_Business_Impact.ipynb` | Revenue impact modelling |
| `outputs/business_impact_model.xlsx` | Excel financial model |

---

## 👤 Author

**[Your Name]**  
[LinkedIn](#) · [Tableau Public](#) · [Medium](#)

---

## 📄 License

MIT — free to use, fork, and adapt.
