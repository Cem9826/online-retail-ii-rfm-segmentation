# Online Retail II — Customer Segmentation & RFM Analysis

End-to-end customer behavior analysis on the Online Retail II dataset using RFM metrics, rule-based segmentation, and an interactive dashboard.

## Project Overview

This project analyzes transaction data from 2009–2011 to segment customers based on Recency, Frequency, and Monetary value. The goal is not only to create segments, but also to validate that the segments behave differently, measure their revenue contribution, and present the results in a decision-ready dashboard.

### Key Results

| Metric | Value |
|--------|-------|
| Net Revenue | 16.4M |
| Total Customers | 5,875 |
| Champions Revenue | 12.5M (~76% of total) |
| Churn Rate | 26.91% |
| Champions Share of Customers | 29.7% |
| Lost Share of Customers | 31.9% |

- **Champions** represent ~30% of customers but generate approximately **76%** of net revenue.
- **Lost** is the largest segment by customer count, yet contributes very little revenue.
- **At Risk** still carries meaningful revenue and represents a clear opportunity for retention actions.

## Data Cleaning Decisions

- Transactions without a Customer ID (guest checkouts) were excluded from RFM analysis, as RFM operates at the customer level.
- **23 customers** who only have return transactions and no positive-quantity purchases were excluded from RFM segmentation (labeled as Only Returns).
- Exact duplicate rows were removed.
- Monetary value is calculated on a net basis (sales minus returns).

## Segmentation Approach

A rule-based segmentation was preferred over pure clustering to keep the labels explainable. Each customer is assigned to exactly one segment. The priority order is:

1. **Return-Heavy** — ReturnIntensity ≥ 0.5 or net Monetary < 0  
2. **Champions** — High Recency + Frequency + Monetary  
3. **Big Spenders** — High Monetary, lower Frequency  
4. **At Risk** — Low Recency, high Monetary  
5. **Loyal** — High Frequency  
6. **Lost** — Low activity  
7. **New** — Recent or low-frequency customers  

A KMeans clustering check was also performed as a structural sanity test to confirm that the rule-based segments capture real patterns in the data.

## Churn Definition

**Official definition used in the project:** A customer is considered churned if they made no purchase in the last 365 days (Recency > 365).

- On the 5,852 customers included in the RFM model, the churn rate is **27.02%**.
- The dashboard reports **26.91%**.  

The small difference occurs because the dashboard uses the full identified customer base of **5,875** (including the 23 Only Returns customers) as the denominator, while the number of customers with Recency > 365 remains essentially the same. This is an intentional reporting choice so that churn is shown against the complete customer base visible in the dashboard.

**Important:** The Lost segment share (31.9%) is a separate metric from churn and should not be interpreted as the churn rate.

## Dashboard

The Looker Studio dashboard includes:

- KPI cards: Net Revenue, Total Customers, Champions Revenue, Churn Rate
- Monthly revenue and order volume trend
- Top countries by revenue
- Top customers by monetary value
- Customer distribution by segment
- Average Recency / Frequency / Monetary by segment
- Net revenue by segment

Filters are available for Year, Country, and Segment.

## Repository Structure

```text
├── notebooks/
│   └── online_retail_ii.ipynb          # Main analysis notebook
├── data/
│   ├── rfm_segments.csv                # Customer-level RFM + Segment
│   ├── rfm_panel.csv                   # Quarterly RFM panel
│   └── dashboard_master.csv            # Transaction-level table with segments
├── images/
│   └── Online_Retail_II.png            # Dashboard screenshot
└── README.md


## License & Legal Attribution

The **Online Retail II** dataset used in this project was obtained from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/502/online+retail+ii).

- **Dataset License:** [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)
- **Terms of Use:** Under the CC BY 4.0 license, you are permitted to copy, process, transform, and use the data for any purpose, including commercial ones, provided that appropriate credit is given to the original creator and a link to the license is included.

*Disclaimer / Notice of Changes: The code in this repository filters, cleans, and transforms the raw UCI dataset for the purpose of customer segmentation and dashboard reporting.*

## Citation

When referencing this work or dataset, you must cite the original author in accordance with CC BY 4.0 as follows:

> Chen, D. (2019). *Online Retail II* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5CG6D
