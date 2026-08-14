# House Price Segmentation & Association Rule Mining

An unsupervised learning case study in R combining clustering, principal component analysis, and association rule mining to identify meaningful property segments and relationships among housing characteristics.

The analysis examines **1,047 properties** across seven variables: living area, bathrooms, bedrooms, lot size, age, fireplace availability, and price.

**[Read the full analysis (PDF)](./Clustering_Analysis_and_Association_Grace_Bianchi.pdf)** | **[View the R Markdown source](./Clustering_Analysis_and_Association_Grace_Bianchi.Rmd)**

## Project Overview

This project approaches housing-market structure from two complementary perspectives:

* **Cluster analysis** groups properties with similar physical and pricing characteristics.
* **Association rule mining** identifies combinations of features that frequently occur together, including patterns associated with different price tiers.

Multiple clustering algorithms were compared to evaluate whether the same broad property segments remained visible across different methods.

## Methods and Results

| Method                    | Purpose                                                    | Main Result                                                          |
| ------------------------- | ---------------------------------------------------------- | -------------------------------------------------------------------- |
| K-means clustering        | Create detailed property segments                          | Elbow analysis supported a three-cluster solution                    |
| Hierarchical clustering   | Evaluate nested group structure                            | Ward.D2 produced compact, interpretable clusters                     |
| Linkage comparison        | Test hierarchical stability                                | Average linkage had the highest cophenetic correlation               |
| PAM clustering            | Evaluate a more robust alternative to k-means              | Silhouette analysis supported a broader two-group structure          |
| PCA                       | Visualize the main dimensions separating properties        | The first two components explained 64.7% of total variance           |
| Hopkins statistic         | Assess whether the data contained cluster structure        | A value of approximately 0.999 indicated strong clustering tendency  |
| Apriori association rules | Identify recurring combinations of housing characteristics | Generated 6,749 rules using 1% support and 30% confidence thresholds |

## Key Findings

### Property Segmentation

The three-cluster k-means solution identified distinct housing profiles:

* **Lower-market homes:** Smaller and older properties with fewer bathrooms, fewer fireplaces, and lower prices.
* **Mid-market homes:** Moderately sized and priced properties with balanced structural features.
* **Higher-market homes:** Larger, newer, and more expensive properties with more bedrooms, bathrooms, and amenities.

PAM clustering supported a broader two-group interpretation, primarily separating larger, higher-value properties from smaller, lower-value properties. This suggests that the housing data has a strong high-versus-low market structure, while k-means provides a more detailed mid-market segment.

### Principal Component Analysis

The first two principal components explained approximately **64.7% of the total variance**:

* **PC1 (49.1%)** represented a size-value dimension driven by living area, lot size, price, bedrooms, and bathrooms.
* **PC2 (15.6%)** primarily contrasted property age with features such as fireplaces and bathrooms.

Projecting the clusters into PCA space showed that the identified segments aligned with meaningful differences in property size, age, amenities, and value.

### Hierarchical Clustering

Four linkage methods were compared using cophenetic correlations:

* Average linkage: **0.760**
* Single linkage: **0.681**
* Complete linkage: **0.634**
* Ward.D2 linkage: **0.523**

Although average linkage preserved the original pairwise distances most closely, Ward.D2 was retained as the primary hierarchical method because it produced compact clusters consistent with the variance-minimizing objective of k-means.

A tanglegram comparison between Ward.D2 and average linkage produced an entanglement value of approximately **0.21**, indicating broadly similar hierarchical structure with relatively limited branch crossing.

### Association Rule Mining

Continuous variables were converted into interpretable categories using quantile-based and feature-specific thresholds. The resulting dataset contained **1,047 transactions** and generated **6,749 association rules**.

Notable patterns included:

* Mid-aged homes with one bathroom and small lots were associated with two-bedroom layouts with approximately **94% confidence** and a lift of about **5.6**.
* Rules involving large living areas, three or more bathrooms, multiple bedrooms, and larger lots were strongly associated with the expensive price category.
* Several expensive-home rules achieved **100% confidence** and a lift of approximately **2.94** within the sample, although these rules had relatively low support.

Together, the clustering and association-rule results revealed consistent differences between smaller, older, lower-priced homes and larger, amenity-rich, higher-priced homes.

## Analysis Workflow

1. Loaded and inspected the housing data.
2. Standardized all variables to make features measured on different scales comparable.
3. Used within-cluster sum of squares and the elbow method to select a k-means solution.
4. Interpreted cluster centroids to develop property-segment profiles.
5. Applied hierarchical clustering with Ward.D2, average, complete, and single linkage.
6. Compared hierarchical solutions using cophenetic correlations and a tanglegram.
7. Used PCA to visualize the property segments and interpret their primary dimensions.
8. Calculated the Hopkins statistic to assess clustering tendency.
9. Applied PAM and silhouette analysis as an alternative clustering approach.
10. Discretized numeric features and converted each property into a transaction.
11. Generated association rules with the Apriori algorithm and evaluated them using support, confidence, and lift.
12. Compiled the code, visualizations, results, and interpretation into a reproducible R Markdown report.

## Tools and Skills

* **R:** data preparation and unsupervised learning
* **factoextra / FactoMineR:** cluster evaluation and PCA visualization
* **cluster:** Partitioning Around Medoids and silhouette analysis
* **dendextend:** dendrogram comparison and tanglegrams
* **hopkins:** evaluation of clustering tendency
* **arules:** Apriori association-rule mining
* **arulesViz:** association-rule visualization
* **ggplot2:** supporting visualizations
* **R Markdown:** reproducible analysis and PDF reporting

## Repository Structure

| File                                                                                                               | Description                                                            |
| ------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------- |
| [`Clustering_Analysis_and_Association_Grace_Bianchi.Rmd`](./Clustering_Analysis_and_Association_Grace_Bianchi.Rmd) | R Markdown source containing the complete analysis                     |
| [`Clustering_Analysis_and_Association_Grace_Bianchi.pdf`](./Clustering_Analysis_and_Association_Grace_Bianchi.pdf) | Rendered 21-page report with code, visualizations, and interpretations |
| [`houseprice.csv`](./houseprice.csv)                                                                               | Housing dataset containing 1,047 properties and seven variables        |

## Data

The included `houseprice.csv` file contains 1,047 property records with the following variables:

| Variable    | Description                          |
| ----------- | ------------------------------------ |
| Living Area | Size of the property's living area   |
| Bathrooms   | Number of bathrooms                  |
| Bedrooms    | Number of bedrooms                   |
| Lot Size    | Size of the property lot             |
| Age         | Age of the property                  |
| Fireplace   | Indicator for fireplace availability |
| Price       | Property price                       |

## Limitations

* Clustering is descriptive and does not establish that particular housing features cause differences in price.
* Price is included as an input to the clustering algorithms, so the resulting segments describe observed price-feature combinations rather than predict prices for new properties.
* Cluster assignments depend on preprocessing choices, the distance metric, and the selected number of clusters.
* K-means assumes approximately spherical clusters and may not fully represent irregular housing-market segments.
* Standardized Euclidean distance is applied to a mixture of continuous, count, and binary variables; alternative measures such as Gower distance may better accommodate mixed data types.
* Association rules depend on the selected discretization boundaries and minimum support and confidence thresholds.
* A 1% support threshold represents approximately 11 properties, so high-confidence rules with low support should not be generalized beyond the sample without additional validation.
* The dataset does not include geographic, neighborhood, renovation-quality, or market-timing variables that could further explain property values.
