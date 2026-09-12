# Ecommerce Customer Analytics: Profitability, RFM Segmentation & Strategic Insights

Build a complete analytical notebook in [CustomerAnalysis.ipynb](file:///home/harsh-raj/Code/ML%20Practice/Data%20Analysis/CustomerAnalysis.ipynb) that answers: **Who are the most profitable customers, what do they buy, and how do we get more of them?**

## Dataset Overview

| File | Rows | Key Columns |
|------|------|-------------|
| `customer_master.csv` | ~25K customers | demographics, acquisition cost, segment |
| `ecommerce_sales_customer_analytics_150k.csv` | ~150K orders | order details, CLV, payment, delivery, reviews |
| `order_items.csv` | ~397K line items | product-level pricing, discounts, profit |
| `product_catalog.csv` | ~1K products | category, brand, supplier, rating |

Data is already loaded and merged into `master_df` (397K rows × 67 columns) in Cell 1.

---

## Proposed Changes

All work continues in [CustomerAnalysis.ipynb](file:///home/harsh-raj/Code/ML%20Practice/Data%20Analysis/CustomerAnalysis.ipynb) — appending new cells after the existing data loading cell.

> [!IMPORTANT]
> Since we are working with a `.ipynb` file, I will provide code blocks in the chat for you to paste and execute cell-by-cell. After each analytical section, I'll add a **markdown findings cell** summarizing the key insight discovered.

> [!IMPORTANT]
> **Visualization approach:** Static charts (matplotlib/seaborn) for final publication-quality visuals; Plotly for exploratory drill-downs requiring hover/zoom.

---

### Section 1: Data Quality & Cleaning Check

**Goal:** Validate the merged dataset before analysis.

- Missing values heatmap per column
- Duplicate row detection on `order_id` + `product_id`
- Data type validation (dates, numerics, categoricals)
- Outlier detection on key financial columns (`net_sales`, `profit`, `discount_percentage`)
- **Findings cell** after this section
- **Actionable Next Steps cell** — what approaches to follow based on data quality results

---

### Section 2: Exploratory Overview (High-Level KPIs)

**Goal:** Set the baseline context with top-line metrics, after applying cleaning rules.

#### 2a. Data Cleaning (Based on Section 1)
- Fill missing `return_status` and `return_reason` with `"No Return"`
- Fill missing `coupon_code` with `"No Coupon"`

#### 2b. KPI Summaries & Distributions
- KPI summary: total revenue, total profit, avg order value, avg profit margin, total customers, total orders
- Distribution plots: order values, profit per order, customer age
- Categorical breakdowns: orders by `sales_channel`, `payment_method`, `customer_country`
- **New additions:** Sales by `gender`, `customer_segment` (VIP, Premium, Consumer), and total sales aggregated by `order_month`
- **Findings cell** summarizing the business landscape
- **Actionable Next Steps cell** — which KPIs warrant deeper investigation

---

### Section 3: Time-Series Trends (2021–2025)

**Goal:** Identify temporal patterns, focusing on the massive Holiday spikes (November/December) discovered in EDA, and overall growth trajectory.

- Monthly revenue & profit trend lines (dual-axis)
- Quarterly order volume bar chart
- Year-over-year growth comparison
- Deep-dive into November/December performance vs rest of year
- **Plotly interactive** chart for drill-down by month
- **[COMPLETED] Findings cell:**
  - **Annual Revenue Baseline Stability:** Net sales remain remarkably stable between **$35.09M (2025)** and **$35.72M (2021)**, with annual order volume holding steady at ~27,500–27,700 orders.
  - **Massive Q4 Holiday Peak:** November ($21.52M sales, 12.15% share) and December ($23.30M sales, 13.15% share) account for **25.30% ($44.82M)** of total sales across the 5-year dataset.
  - **Day-of-Week Uniformity:** Daily sales share across weekdays is almost completely uniform (14.13% to 14.43%), showing steady fulfillment demand across the week without weekend spikes.
- **[COMPLETED] Actionable Next Steps cell:**
  - Target holiday operational efficiency and off-peak Q1 promotional campaigns.
  - Transition directly to **Section 4: RFM Customer Segmentation** to identify which customer sub-groups drive peak sales.

---

### Section 4: RFM Customer Segmentation

**Goal:** Full quintile-based RFM scoring and customer tier classification.

#### 4a. RFM Calculation
- **Recency:** Days since last purchase (relative to max `order_date` baseline)
- **Frequency:** Count of unique `order_id` per customer
- **Monetary:** Total `net_sales` per customer
- Quintile scoring (1–5) for each metric (`pd.qcut` with rank method)
- Combined RFM score and segment label assignment

#### 4b. Segment Labels
Map RFM combinations to tiers:
| Tier | Description | RFM Pattern |
|------|-------------|-------------|
| Champions | Best customers | High R, F, M |
| Loyal Customers | Frequent buyers | High F |
| Potential Loyalists | Recent, moderate spend | High R, moderate F |
| At Risk | Previously good, declining | Low R, high F/M |
| Hibernating | Dormant customers | Low R, F, M |
| Lost | Churned | Lowest R, F, M |

#### 4c. Visualization & Completed Status
- **[COMPLETED] Findings cell:**
  - **High Revenue Concentration:** **Champions (15.57% of customers)** and **Loyal Customers (33.29% of customers)** together generate **68.94% (~$121.66M)** of total company sales!
  - **Re-activation Potential:** **Hibernating customers (20.81%)** represent $27.17M in past value but low recent activity.
  - **Upsell Pool:** **Potential Loyalists (13.62%)** have high recency but lower order count (~$4.57K avg spend), making them ideal for loyalty progression.
- **[COMPLETED] Actionable Next Steps cell:**
  - Establish tier-specific retention and activation strategies.
  - Transition directly to **Section 5: Demographic Overlay on RFM Segments** to understand age, gender, and regional distribution across these tiers.

---

### Section 5: Demographic Overlay on RFM Segments

**Goal:** Profile the ideal high-value customer by cross-referencing RFM tiers with demographics.

- Merge `rfm_df` customer segment tags back into `master_df` / customer profile data.
- Breakdown by `gender`, `customer_age` bins (18-25, 26-35, 36-50, 50+), `region`, `customer_country`.
- Heatmap: RFM segment × Age Group distribution.
- Geographic distribution of Champions vs At-Risk customers.
- `customer_segment` (Consumer / Premium / VIP) overlap with RFM tiers.
- **[COMPLETED] Findings cell:**
  - **Dominant Age Demographic (50+ & 36-50):** Customers aged **50+ represent >40%** of every single RFM segment, while 36-50 accounts for ~26.5%. Together, mature shoppers (36+) make up **>67% of total customers across all segments**.
  - **Gender Independence:** RFM value is evenly split across genders (~48% Female, ~48% Male, ~4% Non-binary across all RFM tiers).
  - **Account Tier vs RFM Behavioral Divergence:** Standard **Consumer account holders make up >54% of RFM Champions**, proving that static account labels (VIP/Premium) under-represent true customer purchasing power.
- **[COMPLETED] Actionable Next Steps cell:**
  - Orient marketing copy and product placement toward 36+ demographic preferences.
  - Transition directly to **Section 6: Product Performance & Profitability** to assess category margins and identify top/bottom products.

---

### Section 6: Product Performance & Profitability

**Goal:** Identify the most and least profitable product lines.

#### 6a. Category-Level Analysis
- Revenue and profit by `product_category` and `product_subcategory`
- Profit margin % comparison across categories (horizontal bar)
- Revenue vs Profit scatter per category (identify high-revenue-low-profit traps)

#### 6b. Product-Level Drill-Down
- Top 10 products by revenue vs Top 10 by profit margin (side-by-side)
- Bottom 10 products by profit (loss-makers)
- Brand-level profitability comparison

#### 6c. Product Rating × Profitability & Next Actionable Steps
- Relationship between `product_rating` and profit margin
- **Findings cell** on product strategy (what to promote, what to reconsider)
- **Actionable Next Steps cell** — inventory and merchandising recommendations + Section 7 Discount Analysis transition

---

### Section 7: Discount Impact Analysis

**Goal:** Find the profitability sweet-spot for discount percentages.

- Bin `discount_percentage` into ranges (0-10%, 10-20%, 20-30%, 30-40%, 40%+)
- Average profit margin per discount bin (bar chart)
- Scatter: discount % vs profit per item
- Discount usage by `product_category` — which categories are over-discounted?
- Compare discount behavior across RFM segments (do Champions need discounts?)
- **Findings cell** recommending optimal discount ranges
- **Actionable Next Steps cell** — discount policy adjustments per category and segment

---

### Section 8: Returns Analysis

**Goal:** Understand return patterns and their profitability impact.

- Overall return rate and trend over time
- Return rate by `product_category` and `product_subcategory`
- Top `return_reason` breakdown (bar chart)
- Profit impact of returns: average profit on returned vs non-returned orders
- Return rate by RFM customer segment (do high-value customers return more?)
- **Findings cell** on categories/segments most affected by returns
- **Actionable Next Steps cell** — return reduction strategies and quality improvement areas

---

### Section 9: Strategic Recommendations (Final Summary)

**Goal:** Synthesize all findings into actionable insights.

- Markdown cell consolidating the inline findings from each section
- Key strategic takeaways organized by: Customer Strategy, Product Strategy, Pricing/Discount Strategy, Operational Improvements

---

## Verification Plan

### Manual Verification
- Execute each code cell sequentially in the notebook
- Verify charts render correctly and communicate insights clearly
- Confirm RFM segments are reasonable (Champions should be a small, high-value group)
- Validate that financial calculations (profit = net_sales - product_cost) are consistent with source data
- Ensure Plotly interactive charts work with hover/zoom
