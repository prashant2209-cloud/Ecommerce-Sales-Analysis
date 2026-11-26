# 🛒 E-Commerce Sales Trend and Customer Analysis
### InternCircle Data Science & AI Internship — Project 03

---

## 📌 Project Introduction

This is **Project 03** of the InternCircle Data Science & AI Internship programme.  
It demonstrates a complete **Business Intelligence and Sales Analytics** workflow on a simulated e-commerce dataset — covering data generation, cleaning, feature engineering, revenue trend analysis, category performance, customer segmentation, Pareto analysis, and strategic business recommendations.

> ⚠️ **Data Disclosure:** The dataset used in this project is **entirely simulated** using NumPy and Pandas with a fixed random seed (`seed=42`). It does **not** represent any real customer transactions, real products, or any real business. It is generated solely for educational and analytical demonstration purposes.

---

## 🎯 Business Objective

> Perform end-to-end analysis of a simulated e-commerce sales dataset to identify monthly revenue trends, top-performing product categories, and the most valuable customers — then convert raw data into actionable business recommendations.

**Three Guiding Business Questions:**
1. **Which months generate the highest and lowest revenue?**
2. **Which product categories perform best in revenue, average order value, and order count?**
3. **Who are the most valuable customers, and what % of total revenue comes from the top 10%?**

---

## 📂 Dataset Description

| Property | Value |
|----------|-------|
| **Type** | Simulated (NumPy/Pandas generated) |
| **Orders** | ~2,000 |
| **Unique Customers** | ~300 |
| **Date Range** | Jan 2023 – Dec 2023 |
| **Categories** | Electronics, Fashion, Home, Books |
| **Target Column** | `amount` (order value in USD) |

### Columns

| Column | Type | Description |
|--------|------|-------------|
| `order_date` | datetime | Date the order was placed |
| `customer_id` | string | Anonymised customer identifier (CUST_XXXX) |
| `category` | string | Product category |
| `amount` | float | Order value in USD |

---

## 🔧 Dataset Generation Methodology

```python
SEED = 42
rng  = np.random.default_rng(SEED)   # reproducible random number generator

# Category price ranges (min, max) in USD
PRICE_RANGES = {
    'Electronics': (200, 1500),   # high-ticket
    'Fashion'    : (30,  400),    # mid-ticket
    'Home'       : (50,  600),    # mid-ticket
    'Books'      : (10,  80),     # low-ticket
}

# Category probability weights (reflects realistic order distribution)
CAT_WEIGHTS = [0.30, 0.35, 0.22, 0.13]   # Electronics, Fashion, Home, Books
```

Order dates are uniformly distributed across the full year. Customer IDs are sampled from a pool of 300 simulated identifiers. Order amounts are drawn from uniform distributions within category-specific price bands — simulating realistic ticket-size differences across categories.

---

## 🛠️ Technologies Used

```python
import pandas as pd       # Data manipulation
import numpy as np        # Numerical simulation
import matplotlib.pyplot as plt  # Base plotting
import matplotlib.ticker as mticker  # Axis formatting
import seaborn as sns     # Statistical charts
```

Install all dependencies:
```bash
pip install -r requirements.txt
```

---

## 🧹 Data Cleaning

| Check | Method | Finding |
|-------|--------|---------|
| `order_date` type | `pd.to_datetime(errors='coerce')` | Converted to datetime64 |
| Missing values | `df.isnull().sum()` | None in simulated data |
| Duplicates | `df.duplicated().sum()` | Structural duplicates exist — kept (different customers) |
| Customer ID format | Regex `^CUST_\d{4}$` | All valid |
| Category names | Set membership check | All valid |
| Amount validity | `pd.to_numeric` + `> 0` filter | All positive |

**Decisions:**
- Rows with unparseable dates → dropped (not applicable in simulated data)
- Rows with non-numeric or negative amounts → dropped
- Duplicate rows → retained (represent distinct orders from different customers)

---

## 🔧 Feature Engineering

Extracted from `order_date` using the Pandas `dt` accessor:

```python
df_clean['year']       = df_clean['order_date'].dt.year
df_clean['month']      = df_clean['order_date'].dt.month
df_clean['month_name'] = df_clean['order_date'].dt.strftime('%b')   # 'Jan', 'Feb', ...
df_clean['weekday']    = df_clean['order_date'].dt.day_name()
df_clean['quarter']    = df_clean['order_date'].dt.quarter
```

`month_name` is converted to an **ordered categorical** so charts and sort operations respect calendar order rather than alphabetical order.

---

## 📈 Monthly Revenue Analysis

```python
monthly_revenue = (
    df_clean.groupby(['month','month_name'], observed=True)['amount']
    .agg(Total_Revenue='sum', Order_Count='count', Avg_Order_Value='mean')
    .sort_values('month')
)
monthly_revenue['MoM_Growth_Pct'] = monthly_revenue['Total_Revenue'].pct_change() * 100
```

**Key metrics:**
- `Total_Revenue` per month via `groupby + sum`
- `pct_change()` for month-over-month growth rates
- Best and worst months identified via `idxmax()` and `idxmin()`

---

## 🏷️ Category Performance Analysis

```python
category_summary = (
    df_clean.groupby('category')['amount']
    .agg(Total_Revenue='sum', Avg_Order_Value='mean', Order_Count='count')
    .sort_values('Total_Revenue', ascending=False)
)
```

**Pivot table** for monthly × category breakdown:
```python
cat_pivot = df_clean.pivot_table(
    values='amount', index='month_name', columns='category', aggfunc='sum'
).fillna(0)
```

**Key findings** (computed from actual data):
- **Electronics** has the highest average order value (~\$850) but fewer orders
- **Fashion** has the highest order volume due to lower price point
- **Books** contributes the least revenue despite moderate order frequency

---

## 👥 Customer Segmentation

```python
customer_summary = (
    df_clean.groupby('customer_id')['amount']
    .agg(Total_Spent='sum', Order_Count='count', Avg_Order='mean')
    .sort_values('Total_Spent', ascending=False)
)
```

Customers sorted by total lifetime spend. Top and bottom spenders identified. Spending distribution analysed to understand revenue concentration.

---

## 📊 Pareto Analysis

The Pareto principle proposes that ~80% of outcomes come from ~20% of inputs.  
We test this empirically:

```python
top10_cutoff      = max(1, int(np.ceil(len(customer_summary) * 0.10)))
top10_revenue     = customer_summary.head(top10_cutoff)['Total_Spent'].sum()
top10_revenue_pct = top10_revenue / customer_summary['Total_Spent'].sum() * 100
```

**Result computed from data** — see notebook for exact percentage.  
A Pareto curve (cumulative % customers vs cumulative % revenue) is plotted for visual verification.

---

## 📈 Visualizations

| File | Description |
|------|-------------|
| `outputs/monthly_revenue.png` | Monthly revenue line chart with best/worst highlighted |
| `outputs/monthly_growth.png` | Month-over-month growth bar chart (green = positive, red = negative) |
| `outputs/category_revenue.png` | Category revenue bar + pie chart |
| `outputs/monthly_category_trend.png` | Stacked bar: monthly revenue by category |
| `outputs/top_customers.png` | Top 10 customers horizontal bar chart |
| `outputs/customer_contribution.png` | Pareto curve: cumulative customer revenue contribution |

---

## 💼 Business Recommendations

Three recommendations derived from actual computed statistics:

### 1. Seasonal Revenue Recovery Campaigns
Run targeted promotions during the lowest-revenue months. Even recovering 10–15% of the best-to-worst revenue gap adds meaningful annual revenue at limited marginal cost.

### 2. Increase Investment in Top Category
Electronics generates the highest revenue share with premium average order values. A 5% revenue lift in this category translates directly to thousands in additional annual revenue.

### 3. Launch VIP Loyalty Programme
The top 10% of customers generate a disproportionate share of total revenue. Retaining these customers through exclusive benefits (early access, free shipping, VIP discounts) increases customer lifetime value and repeat purchase frequency.

> All exact figures are computed in the notebook from the simulated dataset.

---

## ⚠️ Limitations

| Limitation | Detail |
|------------|--------|
| **Simulated data** | No real consumer behaviour, seasonality, or pricing dynamics |
| **Uniform date distribution** | Real e-commerce has seasonal spikes not reflected in uniform sampling |
| **Static categories** | No subcategories, brands, or product-level analysis |
| **No returns/refunds** | Real datasets have cancellation and refund flows that reduce net revenue |
| **No multi-channel** | Excludes mobile app, social commerce, and referral source analysis |
| **Correlation ≠ causation** | Category or seasonal patterns in this data are artefacts of simulation parameters |

---

## 🏁 Conclusion

This project demonstrates a complete, production-grade EDA and business intelligence pipeline:

1. ✅ **Defined business questions first** — guided every analytical decision
2. ✅ **Generated reproducible data** — fixed seed, documented methodology, clear disclosure
3. ✅ **Cleaned rigorously** — validated types, formats, ranges, and duplicates
4. ✅ **Engineered features** — time components unlocked time-series groupby analysis
5. ✅ **Quantified trends** — monthly revenue, MoM growth, category breakdown, Pareto curve
6. ✅ **Delivered actionable recommendations** — every recommendation tied to actual numbers
7. ✅ **Visualized clearly** — six charts, one message per chart, 300 DPI

The analytical framework built here scales directly to real e-commerce datasets.

---

## 📂 Project Structure

```
Ecommerce-Sales-Analysis/
│
├── Ecommerce_Sales_Analysis.ipynb    ← Main Jupyter Notebook
├── README.md                          ← This file
├── requirements.txt                   ← Python dependencies
├── Business_Analysis_Report.md       ← One-page business brief
├── generate_analysis.py              ← Standalone script (runs all analysis)
│
└── outputs/
    ├── monthly_revenue.png
    ├── monthly_growth.png
    ├── category_revenue.png
    ├── monthly_category_trend.png
    ├── top_customers.png
    └── customer_contribution.png
```

---

## 🚀 How to Run

```bash
# 1. Navigate to the project folder
cd Ecommerce-Sales-Analysis

# 2. Install dependencies
pip install -r requirements.txt

# 3. Launch the Jupyter Notebook
jupyter notebook Ecommerce_Sales_Analysis.ipynb

# 4. Run all cells: Kernel → Restart & Run All

# 5. Export to HTML
jupyter nbconvert --to html Ecommerce_Sales_Analysis.ipynb

# Or run the standalone script to generate charts:
python3 generate_analysis.py
```

---

## ☁️ Upload Instructions

### GitHub
```bash
git init
git add .
git commit -m "InternCircle Project 03 — E-Commerce Sales Analysis"
git branch -M main
git remote add origin https://github.com/<your-username>/Ecommerce-Sales-Analysis.git
git push -u origin main
```
Set repository to **Public** for reviewer access.

### Google Drive
1. Upload the full folder to Google Drive.
2. Right-click → **Share → Anyone with the link → Viewer**.
3. Copy and submit the shareable link.

---

*InternCircle Data Science & AI Internship — Project 03*  
*E-Commerce Sales Trend and Customer Analysis | November 2026*
