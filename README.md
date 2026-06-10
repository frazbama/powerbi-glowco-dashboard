# GlowCo — Health, Wellness & Beauty Ecommerce Dashboard
### Power BI Portfolio Project | 2023–2025

> **Tools:** Power BI Desktop · Power Query · DAX · Star Schema Data Modeling  
> **Sectors:** Ecommerce · Digital Marketing · Paid Media Analytics  
> **Data:** 17,775 orders · 5,000 customers · 3 years · 7 paid channels

---

## 🔗 Live Dashboard
[View Interactive Report on Power BI Service](#) ← *add link after publishing*

---

## 📖 The Data Story

GlowCo is a direct-to-consumer health, wellness and beauty brand that launched in 2023.
Despite strong top-line revenue growth over three years, profitability has been under pressure.
This dashboard investigates **why ROAS is declining** even as ad spend increases — and identifies
the product-channel combinations and customer segments that hold the key to sustainable growth.

### Storytelling Arc
| Stage | Business Question |
|-------|------------------|
| **Setting & Hook** | How has GlowCo grown from 2023 to 2025? |
| **Rising Action** | Where is ad spend going — and is it working? |
| **Climax / Aha Moment** | Which channel-category combination drives the highest ROAS & profit? |
| **Falling Action** | Who are the high-LTV customers and how were they acquired? |
| **Resolution** | Where should budget be reallocated to maximize profitable growth? |

---

## 📊 Dashboard Pages

| Page | Type | Purpose |
|------|------|---------|
| **Executive Summary** | Strategic | 3-year revenue, orders, gross profit & ROAS KPIs |
| **Sales Performance** | Operational | Revenue by category, product, region & time |
| **Marketing & Ad Spend** | Analytical | Spend, ROAS, CPA, CTR & CVR by channel |
| **Customer Intelligence** | Analytical | Segments, LTV, acquisition channel & retention |
| **Product Profitability** | Tactical | Margin analysis by category & SKU |
| **Data Explorer** | Exploratory | Raw table view for validation |

---

## 🗂️ Data Model (Star Schema)

```
                    ┌─────────────────┐
                    │   dim_date      │
                    │  (DateKey PK)   │
                    └────────┬────────┘
                             │
┌──────────────┐    ┌────────▼────────┐    ┌──────────────────┐
│ dim_products │◄───│  fact_orders    │───►│  dim_customers   │
│(ProductKey PK)    │  (central fact) │    │ (CustomerKey PK) │
└──────────────┘    └────────┬────────┘    └──────────────────┘
                             │
                    ┌────────▼────────┐
                    │  dim_channels   │◄───┌──────────────────┐
                    │ (ChannelKey PK) │    │  fact_ad_spend   │
                    └─────────────────┘    └──────────────────┘
```

### Tables
| Table | Type | Rows | Description |
|-------|------|------|-------------|
| `fact_orders` | Fact | 17,775 | Order-level transactions with revenue, COGS, discount |
| `fact_ad_spend` | Fact | 1,008 | Weekly ad spend by channel with ROAS, CPA, CTR, CVR |
| `dim_customers` | Dimension | 5,000 | Customer profiles, segments, acquisition channel |
| `dim_products` | Dimension | 31 | Products with category, cost, price, margin |
| `dim_channels` | Dimension | 10 | Marketing channels by type and platform |
| `dim_date` | Dimension | 1,096 | Full date table with year, quarter, month, week |

---

## ⚙️ Key DAX Measures

```dax
-- Total Revenue
Total Revenue = SUMX(fact_orders, fact_orders[Revenue])

-- Gross Profit Margin %
GP Margin % = DIVIDE([Gross Profit], [Total Revenue])

-- Return on Ad Spend
ROAS = DIVIDE([Total Revenue], [Total Ad Spend])

-- Customer Acquisition Cost
CAC = DIVIDE([Total Ad Spend], DISTINCTCOUNT(fact_orders[CustomerKey]))

-- Repeat Purchase Rate
Repeat Rate % = DIVIDE(
    CALCULATE(COUNTROWS(fact_orders), fact_orders[IsRepeatPurchase] = 1),
    COUNTROWS(fact_orders)
)

-- YoY Revenue Growth
Revenue YoY % = 
VAR CY = [Total Revenue]
VAR PY = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(dim_date[Date]))
RETURN DIVIDE(CY - PY, PY)
```

---

## 🔍 Key Insights Uncovered

1. **Supplements category** showed the highest gross margin (avg ~68%) yet received only 18% of ad budget in 2023
2. **Meta Facebook ROAS declined 22%** from 2023 to 2025 while TikTok ROAS improved 31%
3. **High-LTV customers** (top 15%) generated 52% of total revenue — majority acquired via Google Search & Email
4. **Email marketing** had the lowest CPA ($18 avg) of all channels despite being underfunded
5. **November–December** spike accounted for 28% of annual revenue — but also highest return rates

---

## 📁 Repository Structure

```
GlowCo_Ecommerce/
│
├── README.md
├── GlowCo_Dashboard.pbix        ← Power BI source file
├── GlowCo_Dashboard.pdf         ← Static export
│
├── data/
│   ├── fact_orders.csv
│   ├── fact_ad_spend.csv
│   ├── dim_customers.csv
│   ├── dim_products.csv
│   ├── dim_channels.csv
│   └── dim_date.csv
│
└── screenshots/
    ├── 01_executive_summary.png
    ├── 02_sales_performance.png
    ├── 03_marketing_ad_spend.png
    ├── 04_customer_intelligence.png
    └── 05_product_profitability.png
```

---

## 🚀 How to Open This Project

1. Clone or download this repository
2. Open **Power BI Desktop**
3. Go to **Home → Get Data → Text/CSV**
4. Load all 6 CSV files from the `/data` folder
5. Open **Power Query** to apply cleaning steps (see guide below)
6. Build relationships in **Model View** using the star schema above

---

*Part of a 6-project Power BI portfolio covering Ecommerce, Digital Marketing, Dental, Retail, F&B and Ski Resort analytics.*
