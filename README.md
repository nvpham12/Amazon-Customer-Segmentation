# Project Background
This project demonstrates distributed computing, querying, and customer segmentation on big data. K-Means Clustering and RFM (Recency, Frequency, Monetary) Analysis are used to group customers based on spending habits. The K-Means model had questionable performance, so segmentation based on RFM Analysis was done. Data analysis for this project focuses on these segments instead of the clusters.

## Business Objective
Segment customers based on engagement and spending behavior to prioritize retention efforts, optimize marketing spend, and increase customer lifetime value.

## Key Business Questions
- Which customer segments generate the highest value but show churn risk?
- Where should retention budgets be focused for maximum ROI?
- Which segments should receive personalized offers versus broad campaigns?

---

# Executive Summary
## Insights
- **Can’t Lose Them** customers have the highest average monetary value but show declining recency, indicating high-value churn risk.
- **About to Sleep** customers combine moderate recency decline with strong spending, making them the most recoverable at-risk segment.
- Roughly 33% of customers are at risk of churn, while 30% appear fully churned, suggesting retention efforts should prioritize prevention over reactivation.
- A small number of customers drive a disproportionate share of total monetary value.

## Recommendations
- Prioritize retention spend on “Can’t Lose Them” customers, using personalized promotions or Prime trials, as this segment delivers the highest revenue per customer.
- Deploy targeted win-back campaigns for “About to Sleep” customers, such as time-limited discounts or product recommendations based on prior purchase categories.
- Limit retention investment in “Needs Attention” and “At Risk” segments, where low average and total spend suggest lower return on retention cost.
- Use RFM segments to personalize homepage recommendations, testing whether segment-aware personalization improves repeat engagement and spending.

## Proposed Success Metrics
- Retention rate by segment
- Revenue per retained customer
- Campaign response rate by segment
- Repeat purchase frequency

## Limitations
- Reviews may not reflect actual purchase timing or amounts.  
- Only a subset of engaged, verified customers are included.  
- Recency, frequency, and monetary metrics use proxies (review dates and prices) rather than exact purchase data.  

---

# Links
## EDA Jupyter Notebook
- [EDA Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/EDA%20Amazon%20Electronics%20Big%20Data.ipynb)

## K-Means Clustering
- [K-Means Clustering Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/K-Means%20Clustering%20Notebook.ipynb)
- [K-Means Clustering Technical Report](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/K-Means%20Clustering%20Technical%20Report.md)

## RFM Customer Segmentation
- [RFM Segmentation Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/RFM%20Segmentation%20Amazon%20Electronics.ipynb)
- [RFM Segmentation Technical Report](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/RFM%20Segmentation%20Technical%20Report.md)

## Data Analytics
- [Data Analytics Report](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/Data%20Analytics%20Report.md)

---

# Tools & Technologies
- Python (PySpark, SparkSQL, Pandas)
- MLlib
- Matplotlib/Seaborn

---

# Approach
- Ingested and transformed large-scale review and product data using PySpark
- Performed exploratory analysis to understand customer spending and engagement patterns
- Applied RFM analysis and clustering techniques to segment customers
- Analyzed segment behavior to derive retention and marketing insights

---

# Data
Amazon product review and metadata datasets (1996–2023) containing customer interactions, product attributes, and purchase behavior, sourced from the McAuley Lab.

## Data Source
Dataset: Amazon Reviews 2023

Authors: McAuley Lab at University of California, San Diego

Source: [Hugging Face Repository](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023)
