# 🛒 Retail Analytics — Advanced Demand & Sales Insights

> An end-to-end retail analytics project built on **Apache Spark (Databricks)** that covers data cleansing, sales performance, demand forecasting, price elasticity modeling, and product intelligence — using the **UCI Online Retail II** dataset spanning two years of real e-commerce transactions.

---

## 📌 Project Objective

> *"To analyze sales patterns, demand behavior, and price sensitivity across products, and derive actionable pricing and revenue strategies using elasticity-based segmentation."*

The project combines **Sales Analytics** and **Demand & Pricing Analytics** into a unified marketing analytics framework. Sales analysis reveals behavioral patterns across time and geography, while demand analysis drives the pricing strategy — together forming a complete picture for retail decision-making.

---

## 📊 Dataset at a Glance

| Metric | Value |
|--------|-------|
| **Source** | [UCI Online Retail II — Kaggle](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci) |
| **Period** | December 2009 – December 2011 |
| **Total Records** | 1,067,371 transactions |
| **Unique Products** | 5,399 |
| **Total Units Sold** | 11,420,306 |
| **Total Revenue** | £4,246,932 |
| **Countries Covered** | 43 |
| **Most Purchased Product** | WORLD WAR 2 GLIDERS ASSTD DESIGNS |
| **Least Purchased Product** | CRYSTAL SMALL JEWELLED PHOTOFRAME |

**Schema:** `Invoice` · `StockCode` · `Description` · `Quantity` · `InvoiceDate` · `Price` · `Customer ID` · `Country`

> 90% of sales are UK-based; the remaining 10% span 42 international markets.

---

## 📂 Project Structure

```
Retail-Analytics---Advanced-demand-and-sales-insights/
│
├── 1. Data exploration and cleansing.ipynb      # EDA, schema inspection, data cleaning, Delta table creation
├── 2. SALES Analysis.ipynb                      # Revenue trends, top products, country-wise & day-wise sales
├── 3. DEMAND analysis.ipynb                     # Demand patterns, quantity trends, demand distribution
├── 4. Advanced Demand Analysis.ipynb            # Price elasticity, demand segmentation, forecasting
├── 5. Product Analysis.ipynb                    # SKU performance, product rankings, return analysis
│
├── Business document - Insights and Findings.pdf   # Executive summary & business strategy report
├── online-retail-ii-uci.zip                        # Source dataset
└── README.md
```

---

## 🔍 Notebook Breakdown

### 1. 📋 Data Exploration & Cleansing

- Dataset downloaded via **Kaggle API** directly into Databricks workspace
- Loaded using **PySpark** (`spark.read.csv`) with `inferSchema=True`
- Schema: 8 columns — mix of string, integer, double, and timestamp types
- **Null analysis:** `Customer ID` had ~243K missing values; `Description` had ~4.4K nulls
- **Outlier detection:** `Quantity` ranged from -80,995 to +80,995 (negatives = cancellations/returns); `Price` ranged from -53,594 to +38,970
- **Cancellation filtering:** redundant for elasticity and beyond scope of this project — negative `Quantity` and `Price` values are dropped, indicating returns and losses
- Cleaned data saved as a **Delta table** (`retail`) for downstream SQL queries
- Final summary: **5,399 unique products** across **43 countries**

### 2. 💰 Sales Analysis

- Monthly revenue trends with growth rate tracking (2009–2011)
- **Oct–Nov peak** season identified consistently across both years; Jan–Feb shows weakest demand
- Top products by revenue: only ~10 products from 5,399 generated **6-figure revenue** over two years:
  - `REGENCY CAKESTAND 3 TIER` — Rank #1 (£344,563)
  - `Manual`, `DOTCOM POSTAGE`, `WHITE HANGING HEART T-LIGHT HOLDER` follow
- **Peak sales days:** Friday leads, followed by Wednesday and Tuesday; **Sunday is near-zero**; Saturday also very low
- Day 5 (Friday) recorded the highest total sales at **2,377,260 units**

### 3. 📦 Demand Analysis

- Demand broken down by product, country, and time period
- Top product sold per month identified for every month across 2009–2011:
  - `PAPER CRAFT, LITTLE BIRDIE` — 80,995 units in Dec 2011 *(highest single-month quantity)*
  - `MEDIUM CERAMIC TOP STORAGE JAR` — 74,215 units in Jan 2011
- **UK dominates** with 101,353 units for its top product; Denmark is 2nd with 25,164 units

### 4. 🔮 Advanced Demand Analysis & Pricing Strategy

Price elasticity computed using the **percentage change method**, filtered for stability (`|E| ≤ 10`):

| Segment | Elasticity | Behavior | Strategy |
|---------|-----------|----------|----------|
| **Inelastic** | \|E\| < 1 | Stable demand, highest revenue | Cautious price increase — only if \|E\| < 0.5 and demand is high |
| **Elastic** | \|E\| > 1 | Price-sensitive, lowest revenue | Focus on volume — discounts, bundles, promotions |
| **Anomalous (Giffen/Veblen)** | E > 0 | Price and quantity move together | Selective premium pricing for high-revenue items only |

- Revenue is **highly right-skewed** — few products drive outsized gains
- **Anomalous products:** high risk, high reward *(highest variance)*
- **Inelastic products:** low risk, high revenue *(most reliable segment)*
- **Elastic products:** low risk, low revenue *(limited pricing power)*

### 5. 🏷️ Product Analysis

- SKU-level revenue and quantity rankings across all 5,399 products
- Country-wise top products mapped across 43 countries
- Return/cancellation rate analysis by product category
- **Pareto analysis:** elastic products rarely appear in top revenue ranks; anomalous and inelastic categories dominate the top performers

---

## 💡 Key Business Insights

1. **Revenue is primarily driven by inelastic products** — stable demand makes them the most dependable revenue source
2. **Anomalous (Giffen/Veblen) products** offer high but volatile returns — treat as high-risk, high-reward
3. **Elastic products are weak revenue contributors** — compete on volume, not price
4. **Oct–Nov is the critical sales window** — demand builds strongly before peaking, then drops sharply in December *(data anomaly or cut-off)*
5. **Sunday and Saturday sales are negligible** — operational and marketing resources should concentrate Mon–Fri, with Friday as the highest-traffic day
6. **UK is the dominant market** (~90% of all sales); international expansion opportunity exists particularly in Denmark, France, and Germany
7. **A one-size-fits-all pricing strategy is ineffective** — segmented pricing by elasticity category is essential for optimal revenue

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **Apache Spark (PySpark)** | Distributed data processing |
| **Databricks** | Notebook environment & cluster compute |
| **Delta Lake** | Managed table storage for cleaned data |
| **Spark SQL** | Querying and aggregating retail data |
| **Kaggle API** | Automated dataset download |
| **Python 3.12** | Core scripting language |

---

## Getting Started

### Prerequisites
- Databricks workspace *(Community Edition works)*
- Kaggle account + API token (`kaggle.json`)

### Steps

**1. Clone the repository**
```bash
git clone https://github.com/addysiddiqui7/Retail-Analytics---Advanced-demand-and-sales-insights.git
```

**2. Upload notebooks to Databricks**  
Import the `.ipynb` files into your Databricks workspace under a project folder.

**3. Set up Kaggle credentials**
```python
# In your Databricks notebook, configure your Kaggle API key before running notebook 1
import os
os.environ['KAGGLE_USERNAME'] = 'your_username'
os.environ['KAGGLE_KEY'] = 'your_api_key'
```

**4. Run notebooks in order**
```
1 → 2 → 3 → 4 → 5
```
> Notebook 1 creates the `retail` Delta table that all subsequent notebooks depend on.

---

## 📄 Business Report

A complete PDF report (`Business document - Insights and Findings.pdf`) is included, covering:

- Methodology for elasticity computation and segmentation
- Revenue structure analysis across product categories
- Pricing strategy framework with rules per segment
- Risk analysis (high-risk vs. stable segments)
- Final recommendations for pricing and sales operations

> Full analysis with code, queries, and data visualization plots lives in the Databricks notebooks.

---

## 🙋‍♂️ Author

**Adeel Siddique** — [GitHub Profile](https://github.com/addysiddiqui7)

---

> ⭐ If this project was useful or interesting, consider giving it a star!
