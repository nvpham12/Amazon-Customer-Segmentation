# Project Background
This project demonstrates distributed computing, querying, and customer segmentation on big data. K-Means Clustering and RFM (Recency, Frequency, Monetary) Analysis are used to group customers based on spending habits. The K-Means model had questionable performance, so segmentation based on RFM Analysis was done. Data analysis for this project are based on these RFM segments instead of the clusters.

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
- Prioritize retention spend on **Can’t Lose Them** customers, using personalized promotions or Prime trials, as this segment delivers the highest revenue per customer.
- Deploy targeted win-back campaigns for **About to Sleep** customers, such as time-limited discounts or product recommendations based on prior purchase categories.
- Limit retention investment in **Needs Attention** and **At Risk** segments, where low average and total spend suggest lower return on retention cost.
- Use RFM segments to personalize homepage recommendations, testing whether segment-aware personalization improves repeat engagement and spending.

## Proposed Success Metrics
- Retention rate by segment
- Revenue per retained customer
- Campaign response rate by segment
- Repeat purchase frequency

## Project Limitations & Mitigations
- **Review timing:** Reviews aren’t necessarily left at the time of purchase.
  - **Mitigation:** Used review dates from verified customers as a proxy for purchase dates to compute recency.  
- **Volunteer bias:** Not all customers leave reviews; analysis reflects only engaged, reviewing customers.  
- **Frequency calculation:** Does not account for multiple quantities per purchase.  
- **Monetary value estimation:** Prices of reviewed products may not reflect actual amounts paid due to discounts or price changes.
  - **Mitigation:** Used product prices at the time of scraping as a proxy for spending.  
- **Subset of customers:** Only verified reviewers are included. Analysis focuses on this engaged subset.  
- **Missing purchase info:** Data lacks actual purchase amounts and dates.
  - **Mitigation:** Review dates and product prices serve as proxies to approximate recency, frequency, and monetary features for RFM analysis.

---

# Links
## Notebooks
- [EDA Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/EDA%20Amazon%20Electronics%20Big%20Data.ipynb)
- [K-Means Clustering Technical Report](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/K-Means%20Clustering%20Technical%20Report.md)
- [RFM Segmentation Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/RFM%20Segmentation%20Amazon%20Electronics.ipynb)

## Reports
- [K-Means Clustering Technical Report](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/K-Means%20Clustering%20Technical%20Report.md)
- [RFM Segmentation Technical Report](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/RFM%20Segmentation%20Technical%20Report.md)

---

# Tools & Technologies
- PySpark: distributed computing and big data manipulation
- Spark SQL: SQL style querying within PySpark framework
- MLlib: K-Means clustering and unsupervised learning
- Pandas: data manipulation and extracting samples from PySpark dataframe for visualization
- Matplotlib and Seaborn: data visualization

---

# Data
- The data consists of Amazon product user reviews and item metadata between May 1996 to September 2023.
- The reviews dataset contains 43,886,944 rows and 10 columns.
- The product metadata contains 1,610,012 rows and 16 columns.

## Data Source
Dataset: Amazon Reviews 2023

Authors: McAuley Lab at University of California, San Diego

Source: [Hugging Face Repository](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023)

---

# Approach
- An ETL process is used to obtain the data from the Hugging Face Repository, transform it to parquet file type, and load it into PySpark.
- Exploratory Data Analysis and cleaning are done through querying within PySpark.
- K-Means clustering and RFM Analysis are applied for customer segmentation.
- Customer segments and data distributions are visualized and analyzed.

---

# Exploratory Data Analysis
## Popular Products
![2023_popular_products](https://github.com/user-attachments/assets/2c738685-86dd-4cf9-9e15-a9a735bede04)
> Product popularity is approximated using the number of user reviews as a proxy.
- Small electronic devices were the most popular in 2023. Wireless audio devices (earbuds/headphones) were by far the most popular.
- Other popular electronics in 2023 were flash drives and various home improvement products such as cameras and Echo Dots.

---

## Rating Distribution
![rating_distribution](https://github.com/user-attachments/assets/a686da60-1d6a-4fb6-999b-7f49dfa784f8)
- Most product reviews come from users with verified purchases. This was unexpected given that there is no cost requirement (and therefore, a barrier) for users to make reviews without a verified purchases. 
- Users have a tendency to leave ratings at the extreme values (1 and 5).
- Most reviews are 5 star reviews, indicating that users tend to leave a review when they are satisfied with a product.

---

# Customer Segmentation
## RFM Features
Recency, Frequency, and Monetary (RFM) analysis is a method commonly used in marketing and analytics to segment or analyze customers. 
In this project, the RFM features will represent the following:
- **Recency**: how recent the review left by the customer is. 
    
- **Frequency**: how often a customer leaves reviews.

- **Monetary**: how much a customer spends based on prices of the product they reviewed.

## Customer Groups
| Behavior-based Segments | Segments Included                               | Characteristics / Notes                                                            |
|-------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------|
| **High Value / Active** | Champions, Loyal Customers, Potential Loyalists | Recent and frequent purchasers with high spending                                  |
| **New / Low Engagement**| New Customers, Promising                        | Very recent purchasers with low frequency and spending                              |
| **At Risk**             | About to Sleep, At Risk, Can’t Lose Them, Needs Attention | Customers at risk of churning; varying levels of engagement and spending |
| **Churned / Inactive**  | Hibernating, Lost                               | Inactive or churned customers; low engagement and spending                         |

## RFM Segment Counts
<img width="1000" height="600" alt="rfm_segment_count" src="https://github.com/user-attachments/assets/c1e6f055-0cb3-4799-af52-3aae7a9db69c" />


- The segments with the most customers are the **About to Sleep** and **Hibernating** segments, which is bad since these customers are at risk of churn or have churned already. About to Sleep customers haven't churned yet though and may be retained through business strategy.
- The segments with the fewest customers are the **Can't Lose Them** and **At Risk** segments. This is good since a business would want those segments to be least populated.
- There don't appear to be any **New Customers** in the data. This is one of the limitations of using user reviews instead of actual customer purchase information.

---

## RFM Segment Pie
<img width="1000" height="600" alt="rfm_segment_pie" src="https://github.com/user-attachments/assets/5842a65a-881e-4dbd-b9ff-9e3f8817ed5d" />

- 36% of customers are regular or high value customers (**Champions**, **Loyalists**, and **Potential Loyalists**)
- 33% of customers are at risk of churn but haven't churned yet (**About to Sleep**, **At Risk**, **Can't Lose Them**, and **Needs Attention**).
- 30% of customers have already churned (**Lost** and **Hibernating**).

---

## Average Monetary Value by Segment
<img width="1000" height="600" alt="avg_monetary_by_segment" src="https://github.com/user-attachments/assets/075bdf02-53bf-4dbd-9e75-5e205dbc48c2" />

- Based on average Monetary value, customers in the **Can't Lose Them** and **About to Sleep** categories are high value customers that should be targeted for retention.
- **Hibernating**, **Needs Attention**, and **At Risk** customers have the lowest average Monetary value.

---

## Total Monetary Value by Segment
<img width="1000" height="600" alt="total_monetary_by_segment" src="https://github.com/user-attachments/assets/6cea748a-5386-459c-acaa-ca8a4c5bab33" />

- Based on total Monetary value, customers in the **Champions** and **About to Sleep** segments contain the highest value customers.
- **Hibernating**, **Needs Attention**, and **At Risk** customers have the lowest total Monetary value.
- **Can't Lose Them** customers are notable since they have the highest average monetary value but lower total monetary value. This could mean that these customers have few members in the segment, but have high spending per member. This segment has very high value and measures should be taken to retain them.
