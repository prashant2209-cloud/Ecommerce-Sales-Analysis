# 📊 E-Commerce Sales Trend and Customer Analysis
## One-Page Business Brief
### InternCircle Data Science & AI Internship — Project 03

> ⚠️ **Data Notice:** All figures in this report are derived from a **simulated dataset** generated for educational purposes. They do not represent real business performance.

---

## 1. Context

This report presents a full-year (2023) analysis of a simulated e-commerce business with approximately **2,000 orders**, **300 customers**, and **4 product categories**: Electronics, Fashion, Home, and Books.

The analysis was conducted using Python (Pandas, NumPy, Matplotlib, Seaborn) and answers three core business questions:
1. Which months are strongest and weakest for revenue?
2. Which product categories drive the most value?
3. Who are the most valuable customers, and how concentrated is revenue?

---

## 2. Key Findings

### 📅 Monthly Revenue Trends
- **Annual revenue: \$804,862** across 2,000 simulated orders.
- **Best month: April (\$76,262)** | **Worst month: May (\$54,624)** — a gap of **\$21,638**.
- Month-over-month growth swings between **+34.7% (May→Jun)** and **-28.4% (Apr→May)**, indicating high volatility with no sustained trend.
- Average monthly revenue: **\$67,072**.

### 🏷️ Product Category Performance

| Category | Total Revenue | Revenue Share | Avg Order Value | Orders |
|----------|--------------|--------------|----------------|--------|
| **Electronics** 🥇 | **\$511,972** | **63.6%** | \$824 | 621 |
| Fashion 🥈 | \$147,876 | 18.4% | \$211 | 700 |
| Home 🥉 | \$133,261 | 16.6% | \$315 | 423 |
| Books | \$11,753 | 1.5% | \$46 | 256 |

> Electronics alone generates **63.6%** of total revenue despite representing only 31% of orders — driven by its premium average order value of **\$824**.

### 👥 Customer Value — Pareto Analysis
- **300 unique simulated customers**.
- **Top 10% = 30 customers** → generate **\$169,461 (21.1% of total revenue)**.
- The classic 80/20 pattern **does NOT hold** in this dataset — revenue is relatively evenly distributed across the customer base.
- Top customer (CUST_0279): **\$7,439** in annual spending (12 orders).
- VIP threshold: customers spending **>\$4,669** per year qualify for the top 10%.

---

## 3. Recommendations

### Recommendation 1 — Seasonal Revenue Recovery Campaigns 📅
**Finding:** May is the lowest-revenue month (\$54,624), trailing the best month (April: \$76,262) by **\$21,638**.  
**Action:** Run targeted promotions (flash sales, free shipping thresholds, bundle offers) in May and November (second-lowest: \$56,050). Recovering 10% of the gap adds **\$2,164** per weak month; 15% recovery adds **\$3,246**.  
**Expected impact:** \$4,000–\$6,500 in additional annual revenue from promotions in the two weakest months alone.

---

### Recommendation 2 — Increase Investment in Electronics Marketing 🔌
**Finding:** Electronics generates **\$511,972 (63.6% of total revenue)** with an average order value of **\$824** — 4× higher than Fashion.  
**Action:** Increase paid search, display, and affiliate marketing budgets for Electronics by 15–20%. Partner with tech review platforms and YouTube channels. Expand premium product range.  
**Expected impact:** A 5% revenue lift in Electronics alone adds **\$25,599** in annual revenue.

---

### Recommendation 3 — Launch a VIP Loyalty Programme 🌟
**Finding:** The top 30 customers (10%) generate **\$169,461 (21.1% of total revenue)**. The VIP entry threshold is \$4,669 in annual spending.  
**Action:** Create a VIP loyalty tier for customers spending >\$4,669 annually: early product access, free express shipping, exclusive discounts, and a dedicated account contact.  
**Expected impact:** Increasing repeat purchase frequency by just 10% among the top 30 customers adds **\$16,946** in annual revenue at zero customer acquisition cost.

---

## 4. Next Steps

| Priority | Action | Owner |
|----------|--------|-------|
| **High** | Implement campaign calendar for low-revenue months | Marketing Team |
| **High** | Define VIP loyalty tier criteria and launch pilot | CRM / Product Team |
| **Medium** | A/B test increased Electronics ad spend in Q1 | Paid Media Team |
| **Medium** | Investigate Books category: low AOV — consider bundling or subscription model | Merchandising |
| **Low** | Build a live revenue dashboard using this analysis framework | Data Team |

---

> *Report prepared by: InternCircle Data Science Intern*  
> *Date: November 2026*  
> *Analysis tool: Python 3.10, Pandas, Seaborn, Matplotlib*  
> *Dataset: Simulated — for educational purposes only*
