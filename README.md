# Ecommerce Customer Analytics

An end-to-end analysis of ecommerce customers, products, pricing, profitability, discounts, and returns. The project uses the merged order-line dataset to answer:

> Who are the most profitable customers, what do they buy, and how do we get more of them?

## Project Files

- [Customer Analysis Notebook](Data%20Analysis/CustomerAnalysis.ipynb): Complete exploratory and strategic analysis.
- [Detailed Report](detailed_report.md): Markdown report generated from the notebook's markdown cells.
- [Markdown Export Script](Data%20Analysis/export_markdown_cells.py): Regenerates the detailed report from the notebook.
- [Implementation Plan](Data%20Analysis/implementation_plan.md): Analysis scope, section plan, and verification strategy.
- [Progress Tracker](Data%20Analysis/progress_tracker.md): Notebook implementation status.

## Detailed Report Navigation

### Data and Customer Analysis

1. [Data Quality and Cleaning Check](detailed_report.md#-section-1-data-quality--cleaning-check)
2. [Exploratory Overview and KPIs](detailed_report.md#-section-2-exploratory-overview-high-level-kpis)
3. [Time-Series Trends](detailed_report.md#-section-3-time-series-trends-2021-2025)
4. [RFM Customer Segmentation](detailed_report.md#-section-4-rfm-customer-segmentation)
5. [Demographic Overlay on RFM Segments](detailed_report.md#section-5-demographic-overlay-on-rfm-segments)

### Product and Commercial Analysis

6. [Product Performance and Profitability](detailed_report.md#section-6-product-performance--profitability)
7. [Discount Impact Analysis](detailed_report.md#section-7-discount-impact-analysis)
8. [Returns Analysis](detailed_report.md#section-8-returns-analysis)
9. [Strategic Recommendations](detailed_report.md#section-9-strategic-recommendations)

## Key Analytical Areas

- Data quality checks, missing values, duplicate records, data types, and outliers.
- Business KPIs including revenue, profit, order value, customer count, and order count.
- Monthly, quarterly, annual, and holiday-season sales trends.
- Quintile-based RFM segmentation into Champions, Loyal Customers, Potential Loyalists, At Risk, Hibernating, and Lost customers.
- Demographic profiling by age, gender, country, region, and account segment.
- Category, subcategory, brand, and product-level profitability using product names in charts.
- Discount-band profitability and discount behavior across categories and RFM segments.
- Return rates, return reasons, high-return products, and return profitability.

## Regenerating the Report

After changing markdown cells in `CustomerAnalysis.ipynb`, regenerate the report from the project root:

```bash
python3 "Data Analysis/export_markdown_cells.py"
```

The script extracts all non-empty markdown cells in notebook order and writes them to `detailed_report.md`.

## Data Sources

The analysis uses the following files in the [`data`](data) directory:

- [`customer_master.csv`](data/customer_master.csv)
- [`ecommerce_sales_customer_analytics_150k.csv`](data/ecommerce_sales_customer_analytics_150k.csv)
- [`order_items.csv`](data/order_items.csv)
- [`product_catalog.csv`](data/product_catalog.csv)
