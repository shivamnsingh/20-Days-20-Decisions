# 20 Days, 20 Decisions

A data-analytics project series where each day turns a dataset into a business decision, not just a chart. Every analysis ends with what was found, what to do about it, and how confident the conclusion is.

The full plan for the series lives in [`20-Days-20-Decisions_Full_Roadmap.md`](20-Days-20-Decisions_Full_Roadmap.md). It lays out the structure: applied analyses first, then statistical concepts, business metrics, Power BI dashboards, and a capstone case study.

## What's in the repo so far

| Folder | Project | What it shows |
|--------|---------|---------------|
| [`Day01-Used Cars — the accident insight`](<Day01-Used Cars — the accident insight>) | Used car price analysis | Exploratory analysis and a Random Forest on 4,000+ listings. Accident-flagged cars average about $19,961 less than clean ones ($25,862 vs. $45,823). |
| [`Day02-IPL Toss — does it actually matter`](<Day02-IPL Toss — does it actually matter>) | IPL toss advantage | Chi-square hypothesis tests on IPL matches (2008-2024), including venue-level analysis. |
| [`Day03-Product Demand — the accounting bug`](<Day03-Product Demand — the accounting bug>) | Product demand forecasting | Data cleaning (accounting-style negatives like `(1000)`, repeat orders that look like duplicates, missing dates) before building a simple forecast. |
| [`Day04-Startup Success — recall over accuracy`](<Day04-Startup Success — recall over accuracy>) | Startup success prediction | Random Forest with leakage-aware column selection and feature importance, judged on recall rather than accuracy alone. |
| [`Day05-Divorce Prediction — when a model is too good`](<Day05-Divorce Prediction — when a model is too good>) | Divorce prediction | A suspiciously accurate model: feature selection done after the train/test split, correlation analysis, and AdaBoost. |
| [`Day08-AB Testing`](<Day08-AB Testing>) | A/B testing | Cleaning an experiment dataset (mismatched groups, duplicate users) and running a two-proportion z-test on conversion rates. |
| [`RFM-MODEL`](RFM-MODEL) | Customer lifetime value | BG-NBD and Gamma-Gamma models on the UCI Online Retail II dataset (UK customers) to forecast 6-month customer value. |

Each project folder contains its notebook and the data it uses. Some folders also include saved charts.

## Getting started

```bash
git clone https://github.com/shivamnsingh/20-Days-20-Decisions.git
cd 20-Days-20-Decisions
pip install jupyter pandas numpy matplotlib seaborn scikit-learn scipy lifetimes openpyxl
jupyter notebook
```

Open the notebook inside any project folder and run it top to bottom. Launch Jupyter from the repository root so the relative data paths work.

**Note on data size:** some datasets are large (the product demand CSV is about 50 MB and `online_retail_II.xlsx` is about 45 MB), so cloning may take a moment.

## Tech stack

Python, pandas, NumPy, scikit-learn, SciPy, Matplotlib, Seaborn, and `lifetimes` for customer lifetime value modeling.
