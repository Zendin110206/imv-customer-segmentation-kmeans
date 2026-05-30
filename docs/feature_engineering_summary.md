# Feature Engineering Summary

## Scope

This document summarizes the customer-level feature engineering step for the Olist customer segmentation project.

## Base Scope

The base feature table uses delivered orders only. Delivered orders are selected because they represent completed purchases and support delivery-related feature calculation.

## Tables Used

| Table                  | Usage                                                                          |
| ---------------------- | ------------------------------------------------------------------------------ |
| `customers`            | Attach `customer_unique_id` to orders.                                         |
| `orders`               | Define completed purchases, purchase dates, delivery dates, and late delivery. |
| `order_items`          | Build item count, product count, item price, and freight features.             |
| `order_payments`       | Build payment value, installment, and payment method features.                 |
| `order_reviews`        | Build review score features.                                                   |
| `products`             | Add product category information.                                              |
| `category_translation` | Translate product categories when available.                                   |

## Tables Not Used in Base Feature Set

| Table         | Reason                                                                                                            |
| ------------- | ----------------------------------------------------------------------------------------------------------------- |
| `geolocation` | Optional location table with many duplicate rows. It is not required for the core purchase behavior segmentation. |
| `sellers`     | Seller-side information is not part of the first customer behavior feature set.                                   |

## Main Feature Groups

| Feature Group       | Example Features                                                                            |
| ------------------- | ------------------------------------------------------------------------------------------- |
| Recency             | `recency_days`                                                                              |
| Frequency           | `order_count`                                                                               |
| Monetary            | `total_payment_value`, `avg_payment_value`                                                  |
| Payment behavior    | `avg_payment_installments`, `avg_payment_method_count`                                      |
| Product behavior    | `total_items`, `avg_items_per_order`, `total_product_count`, `max_category_count_per_order` |
| Freight behavior    | `total_freight_value`, `avg_freight_value`, `freight_to_payment_ratio`                      |
| Satisfaction        | `avg_review_score`, `review_count`                                                          |
| Delivery experience | `avg_delivery_days`, `late_delivery_rate`                                                   |

## Output

The local processed feature table is saved to:

`data/processed/customer_features_base.csv`

This file is ignored by Git and is not committed to the repository.

## Known Issues for Next Step

- Some customers do not have review score.
- One customer/order path has missing payment-derived values.
- Monetary and freight-related features are likely skewed.
- Feature selection, missing value handling, skew handling, and scaling are still required before K-Means.
