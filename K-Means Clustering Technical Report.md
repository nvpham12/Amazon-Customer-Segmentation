# Table of Contents
- [Project Background](#project-background)
- [Data Cleaning](#data-cleaning)
- [RFM Feature Baseline](#rfm-feature-baseline)
- [K-Means Clustering](#k-means-clustering)
- [Next Steps](#next-steps)
- [Appendix](#appendix)

---

# Project Background
This project demonstrates distributed computing, querying, and customer segmentation on big data. K-Means Clustering and RFM (Recency, Frequency, Monetary) Analysis are used to group customers based on spending habits.
This report focuses on the K-Means Clustering Model used for segmentation.

## Project Limitations
- **Volunteer bias**: Not all customers leave reviews; analysis reflects only engaged, reviewing customers.
- **Frequency calculation**: Does not account for multiple quantities per purchase.
- **Subset of reviewers**: Reviews are left by users and those who haven't made a verified purchase may not have bought the item being reviewed.
    - **Mitigation** Only verified reviewers are included, ensuring that only verified customers are selected from users. Analysis focuses on this engaged subset.
- **Review timing**: Reviews aren’t necessarily left at the time of purchase.
    - **Mitigation**: Used review dates from verified customers as a proxy for purchase dates to compute recency.
- **Monetary value estimation**: Prices of reviewed products may not reflect actual amounts paid due to discounts or price changes.
    - **Mitigation**: Used product prices at the time of scraping as a proxy for spending.
- **Missing purchase info**: Data lacks actual purchase amounts and dates.
    - **Mitigation**: Review dates and product prices serve as proxies to approximate recency, frequency, and monetary features for RFM analysis.

---

# Data Cleaning
- Converted missing value strings such as 'none' and 'n/a' into nulls.
- Dropped duplicates.
- Filtered data to last 2 years of observations (2022 and 2023) and only reviews with verified purchases.
- Removed incorrect observations and columns.
- Converted data types to proper format such as timestamp to seconds and price to double (a type of integer in PySpark).

---

# RFM Feature Baseline
Recency, Frequency, and Monetary (RFM) analysis is a method commonly used in marketing and analytics to segment or analyze customers. RFM will be computed as features and the estimated scores for RFM features will be uses as a baseline for clustering. 
The features represent the following:

- **Recency**: how recent a customer made a purchase.
    
- **Frequency**: how often a customer makes purchases.

- **Monetary**: how much a customer spends.

The RFM features will represent the following in this project:
- **Recency**: how recent the review left by the customer is. 
    
- **Frequency**: how often a customer leaves reviews.

- **Monetary**: how much a customer spends based on prices of the product they reviewed.

---

# K-Means Clustering
 K-Means Clustering, an unsupervised machine learning algorithm. K-Means initializes a selected k number of points called means or centroids. It then finds the nearest distances between each data point the nearest centroid (using Euclidean distance) updating the positions of centroids. The process is repeated until we have unchanging centroids or cluster assignments, or until we have reached the maximum number of iterations. Before fitting the model, K needs to be chosen. 2 commonly used techniques for this are the Elbow Method and Silhouette Method.

## Data Preprocessing
- The data is filtered further to a 1 year range between 9/13/2022 to 9/13/2023 based on the date of the most recent review in the data.
- Applied a log transformation on skewed features.
- Scaled the data using Standard Scaler.

## Elbow Method
![WCSS Elbow Method Chart](<Visuals/WCSS.png>)
- The Elbow Method involves plotting within-cluster sum of squares (WCSS) against k and looking for the elbow, the point where the curve forms a kink or angle at some k.
- The angles appear in the curve at values of 3 and 4 for k.
- Since the curve appears linear after 4, choosing $k=4$ is the better option per Elbow Method guidlines.

## Silhouette Method
![silhoutte_score](<Visuals/Silhoutte Score.png>)
- The value of $k$ with the highest Silhouette Score is 2.
- The Silhouette Scores where $k=4$ and $k=6$ are lower, but not too far off from the score for $k=2$.
- Choosing $k=2$ would be best, but $k=4$ and $k=6$ can be chosen as well per Silhouette Method guidelines.

## Choice of k Clusters
From analysis of results from both methods, we would choose $k=4$ clusters, since it is the best choice from the Elbow Method and the Silhouette Score is not too far off from the highest score at $k=2$.

## Clusters and Segments
Clusters were labeled based on analysis of the average values for each RFM metric in each cluster. The 4 clusters are Churned, New Customers, Loyal Customers, and One Time Big Spenders. 

![Customer Segments](<Visuals/Segments.png>)
- **New Customers** make up around 30% of the total customers. 
- **Churned Customers** and **One Time Big Spenders** sum up to around half the total customers.
- **Loyal Customers** make up 15% of total customers and is the segment with the fewest customers.

## Validation
- The clusters are not significantly imbalanced and cluster sizes are acceptable.
- The Silhouette Score for the clustering is around 0.49, which is moderate clustering quality and indicates room for improvement.
- The true optimal k may have been a higher value like 8 or 10, but such values were not tested due to high computational requirements and runtime. Common practice is to select a value for $k$ between 2 to 6.

---

# Next Steps
- Test the K-Means model with more clusters to evaluate whether deeper segmentation yields more interpretable or actionable groupings.
- Explore other algorithms for customer segmentation and compare their performance with the K-Means model.
- Perform sentiment analysis on review text and map sentiment trends to customer segments for richer behavioral insight.

---

# Appendix
<details>
<summary><b>
Click to see links, tools used, and data source.
</b></summary>
<br></br>


# Links
## EDA Jupyter Notebook
- [EDA Notebook](./EDA.ipynb)

## K-Means Clustering
- [K-Means Clustering Notebook](./K-Means%20Clustering.ipynb)

## RFM Customer Segmentation
- [RFM Segmentation Notebook](./RFM%20Segmentation.ipynb)
- [RFM Segmentation Technical Report](./RFM%20Segmentation%20Technical%20Report.md)

## Data Analytics
- [Data Analytics Report](./Data%20Analytics%20Report.md)

# Tools & Technologies
- **PySpark**: distributed computing and big data manipulation
- **Spark SQL**: SQL style querying within PySpark framework
- **MLlib**: K-Means clustering and unsupervised learning
- **Pandas**: data manipulation and extracting samples from PySpark dataframe for visualization
- **Matplotlib and Seaborn**: data visualization

# Data
- The dataset contains information Amazon product user reviews and item metadata between May 1996 to September 2023.
- The product reviews dataset has 43,886,944 rows and 10 columns.
- The product metadata dataset has 1,610,012 rows and 16 columns.

# Data Source
Dataset: Amazon Reviews 2023

Authors: McAuley Lab at University of California, San Diego

Source: [Hugging Face Repository](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023)

Reference: Hou, Y., Li, J., He, Z., Yan, A., Chen, X., & McAuley, J. (2024). Bridging language and items for retrieval and recommendation. arXiv preprint arXiv:2403.03952. https://arxiv.org/abs/2403.03952
</details>