# Preprocessing Summary

## Scope

This document summarizes feature validation and preprocessing for the Olist customer segmentation project.

The goal of this step is to prepare a clean, numeric, scaled feature matrix for K-Means clustering. K-Means is not trained in this step.

## Input

Input file:

`data/processed/customer_features_base.csv`

The input file contains one row per `customer_unique_id` and was created from delivered Olist orders.

## Selected Features

| Feature                    | Behavior Group      | Reason                                                          |
| -------------------------- | ------------------- | --------------------------------------------------------------- |
| `recency_days`             | Recency             | Measures how recently the customer purchased.                   |
| `order_count`              | Frequency           | Measures how often the customer purchased.                      |
| `total_payment_value`      | Monetary            | Measures total completed purchase value.                        |
| `avg_payment_installments` | Payment behavior    | Captures installment usage behavior.                            |
| `total_product_count`      | Product behavior    | Captures product variety across completed purchases.            |
| `freight_to_payment_ratio` | Freight behavior    | Measures shipping cost burden relative to payment value.        |
| `avg_review_score`         | Satisfaction        | Captures average customer review score when available.          |
| `avg_delivery_days`        | Delivery experience | Measures typical delivery duration.                             |
| `late_delivery_rate`       | Delivery experience | Measures how often the customer's orders were delivered late.   |
| `has_review_score`         | Satisfaction signal | Preserves whether review information existed before imputation. |

## Missing Value Handling

Missing values are handled with median imputation for:

- `avg_payment_installments`
- `freight_to_payment_ratio`
- `avg_review_score`

The `has_review_score` indicator is created before imputation so the model can still distinguish customers with actual review information from customers whose review score was imputed.

## Transformation

The following non-negative skewed features are transformed using `np.log1p`:

- `recency_days`
- `order_count`
- `total_payment_value`
- `avg_payment_installments`
- `total_product_count`
- `freight_to_payment_ratio`
- `avg_delivery_days`

`np.log1p` is used because it handles zero values safely.

## Scaling

All final model features are scaled using `StandardScaler`.

Scaling is required because K-Means is distance-based. Without scaling, high-value features such as payment amount could dominate lower-scale features such as review score or late delivery rate.

## Output

Local processed outputs:

- `data/processed/customer_features_preprocessed.csv`
- `data/processed/customer_features_model_ready.csv`

These files are ignored by Git because they are reproducible processed data.

Committed supporting figures:

- `reports/figures/missing_values_selected_features.png`
- `reports/figures/payment_value_log_transform.png`
- `reports/figures/model_feature_correlation_heatmap.png`

## Known Notes for Clustering

- Some features remain skewed because most customers only purchased once.
- Binary or near-binary features such as `has_review_score` can remain imbalanced. This is expected and should be interpreted carefully.
- The next step should compare multiple K values using inertia, silhouette score, Davies-Bouldin score, cluster size balance, and business interpretability.
