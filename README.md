# Superstore Profitability, Customer Insights & Sales Forecasting

A portfolio business analytics project that converts retail transaction data into management insights. The project combines profitability diagnostics, discount analysis, customer and geographic analysis, a transparent six-month sales forecast, and an interactive Power BI dashboard.

> **Data source:** Kaggle Sample Superstore dataset (`vivek468/superstore-dataset-final`). This is a sample dataset used for analytical demonstration; it does not represent a real company.

## Business questions

1. Which categories, sub-categories, regions, cities, and customers generate profit or conceal loss exposure?
2. How is discounting associated with transaction-line profit, conditional on category, region, and customer segment?
3. Which segment drives the observed October sales dip?
4. What sales range should management use for six-month directional planning?
5. Which management actions should be prioritized, and how should success be measured?

## Headline findings

- The business generates approximately **$2.30M in sales** and **$286.1K in profit**, a **12.46% profit margin**.
- **18.7% of transaction lines are loss-making**.
- Three sub-categories—**Tables, Bookcases, and Supplies**—have negative aggregate profit in the Power BI model.
- A separate loss-line analysis finds that **Binders, Tables, Machines, and Bookcases** account for **72.5% of loss dollars** among transaction lines with negative profit. This is a different metric from aggregate sub-category profitability.
- Average transaction-line profit is positive at a 20% discount and negative at the observed 30% discount tier.
- In an OLS model with HC3 robust standard errors, a 10-percentage-point higher discount is associated with about **$25 lower transaction-line profit**, conditional on category, region, and segment.
- **155 of 793 customers** have negative aggregate profit. The top 10 customers contribute **17.5% of total profit**, indicating that profit is not strongly concentrated in a handful of accounts.
- Philadelphia is a high-sales but loss-making city: approximately **$109.1K sales** and **−$13.8K profit**.
- A six-month forecast validated on Jul–Dec 2017 has **19.0% MAPE**. The Jan–Jun 2018 base-case projection is approximately **$305K**, with a validation-error-based planning range of approximately **$247K–$363K**.

## Dashboard

The Power BI report is structured as a management-facing workflow:

| Page | Purpose |
|---|---|
| Overview | Headline KPIs, category/region profit, seasonality, and high-level performance drivers |
| Forecast | Six-month sales forecast, holdout validation, and planning scenarios |
| Profitability | Discount risk, loss concentration, regression result, and profit decomposition |
| Customers & Geography | Segment, regional, monthly-margin, and city profitability analysis |
| Management Action Plan | Prioritized and measurable business actions |
| Customer Deep Dive | Customer-level sales/profit distribution and loss-making customer detail |

Add exported dashboard screenshots to `dashboard/screenshots/` before publishing the repository.

## Methodology

### Units of analysis

The raw dataset contains **transaction-line records**. The project keeps analytical levels explicit:

- Product, category, sub-category, discount-tier, and transaction-line profit analysis use the **transaction-line level**.
- Order count and average order value use distinct **`Order ID`** values.
- Customer profitability uses profit aggregated by **`Customer ID`**.

### Profitability and discount analysis

- Descriptive analysis compares sales, profit, margin, and average discount by category, sub-category, region, city, and segment.
- Discount tiers are used to identify where average transaction-line profit turns negative.
- OLS regression estimates the conditional association between profit and discount:

\[
Profit_i = \beta_0 + \beta_1 Discount_i + Category_i + Region_i + Segment_i + \varepsilon_i
\]

- HC3 heteroskedasticity-robust standard errors are used.
- The regression is **associational**, not causal: the dataset lacks item-level COGS, promotion type, marketing spend, inventory pressure, and negotiated customer terms.

### Forecasting

- Monthly sales are aggregated from January 2014 through December 2017.
- The selected model combines a linear trend with median monthly seasonal indices.
- Training window: Jan 2014–Jun 2017 (42 months).
- Validation window: Jul 2017–Dec 2017 (6 months).
- A log-linear alternative was tested and produced higher MAPE (25.4% versus 19.0%).
- The optimistic and pessimistic scenarios use the validated MAPE as a planning-band width; they are **not formal statistical prediction intervals**.

## Repository structure

```text
superstore-profitability-analysis/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── README.md
├── notebooks/
│   ├── 01_profitability_analysis.ipynb
│   └── 02_rolling_sales_forecast.ipynb
├── dashboard/
│   ├── Superstore_Dashboard.pbix
│   ├── screenshots/
│   │   ├── overview.png
│   │   ├── forecast.png
│   │   ├── profitability.png
│   │   ├── customers_geography.png
│   │   ├── action_plan.png
│   │   └── customer_deep_dive.png
│   └── dashboard_notes.md
└── reports/
    └── Superstore_Controlling_Report.pdf
```

## Reproducibility

### 1. Clone the repository

```bash
git clone https://github.com/<your-github-username>/superstore-profitability-analysis.git
cd superstore-profitability-analysis
```

### 2. Create an environment and install packages

```bash
python -m venv .venv
```

macOS/Linux:

```bash
source .venv/bin/activate
```

Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
pip install -r requirements.txt
```

### 3. Obtain the data

Download the Sample Superstore CSV from the Kaggle data source above and place it in the `data/` directory. The raw dataset is not included in this repository unless its license permits redistribution.

### 4. Run the notebooks

Run the notebooks in this order:

```text
01_profitability_analysis.ipynb
02_rolling_sales_forecast.ipynb
```

In Kaggle, use **Run All** or **Restart Session and Run All**. This ensures imports and upstream objects—such as `df`, `subcategory_summary`, `smf`, `future`, and `forecast_results`—exist before downstream cells run.

## Key limitations

- The data is a sample dataset, not operational company data.
- The `Profit` field does not expose COGS, freight cost, marketing spend, inventory costs, or promotion costs.
- The discount regression identifies conditional association, not causal effect.
- The explanation of the October Consumer dip identifies the segment and mechanics of the decline, not its external cause.
- The forecast uses only 48 monthly observations and is suitable for directional planning rather than hard revenue commitments.

## Tools

- Python: pandas, NumPy, matplotlib, seaborn, statsmodels, scikit-learn
- Power BI: data model, DAX measures, interactive visuals, management dashboard
- Kaggle: notebook execution and source dataset

## Author

Xing LIN
