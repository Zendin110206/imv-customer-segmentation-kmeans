# K-Means Evaluation Summary

## Scope

This document summarizes the K-Means modeling and customer segment interpretation step for the Olist customer segmentation project.

The goal is to compare several cluster counts, select a practical final K, and interpret the customer segments from a business perspective.

## Input

Input file:

`data/processed/customer_features_model_ready.csv`

The input file contains one row per `customer_unique_id` and 10 scaled numeric features.

## Evaluation Setup

| Setting                | Value              |
| ---------------------- | ------------------ |
| Algorithm              | K-Means Clustering |
| K values tested        | 2 to 8             |
| Random state           | 42                 |
| n_init                 | 20                 |
| Silhouette sample size | 10,000             |

Accuracy is not used because this is an unsupervised learning project.

## Final K Selection

The selected cluster count is **K=4**.

K=2 has the highest silhouette score, but it is not selected because one cluster contains about 96.65% of customers. This is too broad for practical customer segmentation. K=3 is also still highly imbalanced. K=5 and above introduce very small clusters, including a cluster strongly related to review availability, which adds complexity for the main assignment scope.

K=4 provides a better balance between metric evaluation, cluster size distribution, and business interpretability.

## Final Customer Segments

| Cluster | Segment Name                                          | Customer Count | Customer Share |
| ------- | ----------------------------------------------------- | -------------: | -------------: |
| 0       | Low-value one-time customers with high freight burden |         42,112 |         45.11% |
| 1       | Higher-value installment one-time customers           |         40,718 |         43.62% |
| 2       | Delayed and dissatisfied customers                    |          7,401 |          7.93% |
| 3       | Repeat and broader-basket customers                   |          3,119 |          3.34% |

## Segment Interpretation

### Low-value one-time customers with high freight burden

This segment contains mostly one-time buyers with low median payment value and relatively high freight-to-payment burden. These customers may be price-sensitive and may be affected by shipping costs.

Recommended action:

- Use shipping threshold offers.
- Promote bundles that make shipping feel more worthwhile.
- Run low-risk reactivation campaigns.

### Higher-value installment one-time customers

This segment contains mostly one-time buyers with higher payment value and higher installment usage. These customers may respond well to payment flexibility and higher-value offers.

Recommended action:

- Promote installment-friendly offers.
- Recommend premium bundles.
- Use personalized product recommendations after the first purchase.

### Delayed and dissatisfied customers

This segment has lower review scores, longer delivery duration, and high late delivery rate. The main business issue is customer experience, especially delivery reliability.

Recommended action:

- Improve delivery reliability.
- Communicate proactively when delays happen.
- Offer service recovery for delayed orders.
- Monitor this segment carefully because poor delivery experience can reduce retention.

### Repeat and broader-basket customers

This segment contains repeat buyers with higher median payment value and more product variety. This is the strongest loyalty-oriented segment in the current feature set.

Recommended action:

- Use loyalty rewards.
- Offer cross-selling recommendations.
- Give early access to relevant offers.
- Prioritize retention campaigns.

## Supporting Figures

- `reports/figures/kmeans_elbow_curve.png`
- `reports/figures/kmeans_metric_comparison.png`
- `reports/figures/kmeans_cluster_balance.png`
- `reports/figures/customer_segment_size_distribution.png`
- `reports/figures/customer_segment_business_kpi_summary.png`
- `reports/figures/customer_segments_pca.png`
- `reports/figures/customer_segments_pca_3d.png`
- `reports/figures/customer_segment_profile_heatmap.png`

## Report Tables

- `reports/tables/kmeans_evaluation_metrics.md`
- `reports/tables/customer_segment_profiles.md`
- `reports/tables/segment_interpretation.md`

## Known Limitations

- The segment labels are based on historical Olist data and should be interpreted as analytical segments, not permanent customer identities.
- PCA visualization is only a reduced-dimensional projection and does not capture all variation in the 10-dimensional feature space.
- K-Means assumes compact distance-based clusters, so future portfolio improvements can compare other clustering methods after the IMV assignment is complete.
