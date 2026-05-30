# Raw Data Audit

## Audit Scope

This document summarizes the initial raw data inspection for the Olist customer segmentation project. The audit checks table size, missing values, duplicate rows, key uniqueness, relationship consistency, and early business-relevant distributions before preprocessing and K-Means clustering.

## Raw Table Inventory

| Table                |      Rows | Columns | Duplicate Rows | Main Role                                                   |
| -------------------- | --------: | ------: | -------------: | ----------------------------------------------------------- |
| customers            |    99,441 |       5 |              0 | Customer identity and location.                             |
| orders               |    99,441 |       8 |              0 | Order status and timestamps.                                |
| order_items          |   112,650 |       7 |              0 | Item-level product, seller, price, and freight information. |
| order_payments       |   103,886 |       5 |              0 | Payment method, installment, and payment value.             |
| order_reviews        |    99,224 |       7 |              0 | Review score and review text fields.                        |
| products             |    32,951 |       9 |              0 | Product category and product attributes.                    |
| category_translation |        71 |       2 |              0 | Product category translation.                               |
| geolocation          | 1,000,163 |       5 |        261,831 | Optional geographic reference table.                        |
| sellers              |     3,095 |       4 |              0 | Optional seller reference table.                            |

## Important Missing Value Findings

- `orders` has missing delivery-related timestamps, especially for non-delivered orders.
- `order_reviews` has many missing review comment titles and messages. This is acceptable for the current plan because the main planned satisfaction signal is `review_score`.
- `products` has missing product category and product attribute values.
- Missing values should not be removed blindly. Each missing pattern must be handled based on its business meaning and feature usage.

## Key and Relationship Findings

- `customer_id` is unique in the customers table.
- `customer_unique_id` is not unique because it represents the same customer across possible multiple orders.
- `order_id` is unique in the orders table.
- Main foreign-key relationships between orders, items, payments, reviews, products, and sellers are consistent.
- Two product categories do not have English translation: `pc_gamer` and `portateis_cozinha_e_preparadores_de_alimentos`.

## Order Status Findings

Most orders are delivered:

| Status      |  Count |
| ----------- | -----: |
| delivered   | 96,478 |
| shipped     |  1,107 |
| canceled    |    625 |
| unavailable |    609 |
| invoiced    |    314 |
| processing  |    301 |
| created     |      5 |
| approved    |      2 |

The purchase timestamp range is from `2016-09-04 21:15:19` to `2018-10-17 17:30:18`.

## Payment and Review Findings

- Credit card is the most frequent payment type.
- Payment value and item price are likely skewed and should be inspected before clustering.
- Review score is concentrated at score 5, but low review scores are still present and useful for customer experience interpretation.

## Modeling Implications

The dataset is suitable for customer segmentation, but feature engineering must be done carefully:

- Use `customer_unique_id` as the customer-level identity.
- Use raw IDs only for joins, not as clustering features.
- Consider focusing on delivered orders when creating delivery-related features.
- Treat missing review text as non-critical unless text analysis is added later.
- Inspect skewed numerical features before scaling and K-Means.
- Avoid using the geolocation table unless it clearly improves the segmentation objective.

## Next Step

The next checkpoint should create a clean customer-level analytical dataset by joining relevant tables and engineering behavior features such as recency, frequency, monetary value, payment behavior, delivery experience, review score, and product diversity.
