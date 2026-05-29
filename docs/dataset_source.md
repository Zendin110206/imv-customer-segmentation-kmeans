# Dataset Source

## Dataset Identity

Dataset name: Olist Brazilian E-Commerce Public Dataset  
Dataset source: <https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce>  
Kaggle dataset slug: `olistbr/brazilian-ecommerce`  
Project usage: customer segmentation using K-Means clustering for IMV Recruitment 2026.

## Why This Dataset Is Used

This dataset is used because it supports customer behavior analysis from multiple perspectives, including order frequency, transaction value, payment behavior, delivery experience, review score, and product diversity.

Compared with a simple toy customer dataset, the Olist dataset is richer and more realistic. It allows the project to build customer-level features from several raw transaction tables before applying K-Means clustering.

## Required Raw Files

The following raw files are required for the main analysis:

| File                               | Planned Usage                                                                     |
| ---------------------------------- | --------------------------------------------------------------------------------- |
| `olist_customers_dataset.csv`      | Connect customer IDs and customer unique IDs.                                     |
| `olist_orders_dataset.csv`         | Analyze order status and order timestamps.                                        |
| `olist_order_items_dataset.csv`    | Calculate transaction value, product count, freight value, and product diversity. |
| `olist_order_payments_dataset.csv` | Analyze payment value, payment type, and installment behavior.                    |
| `olist_order_reviews_dataset.csv`  | Add customer satisfaction signal through review score.                            |
| `olist_products_dataset.csv`       | Add product category and product attribute context.                               |

## Optional Raw Files

The following files may be used if they add clear analytical value:

| File                                    | Possible Usage                                              |
| --------------------------------------- | ----------------------------------------------------------- |
| `product_category_name_translation.csv` | Translate product category names for easier interpretation. |
| `olist_geolocation_dataset.csv`         | Add geographic context if needed.                           |
| `olist_sellers_dataset.csv`             | Add seller-related context if needed.                       |

Optional files should not be added only to make the project look complex. They should be used only when they improve the customer segmentation analysis or interpretation.

## Data Storage Rule

Raw dataset files are stored locally in:

`data/raw/`

The raw CSV files are not committed to GitHub. They are ignored through `.gitignore` because they are external data files that can be downloaded again from Kaggle.

## Reproducibility Note

To reproduce the project, download the dataset from Kaggle using the dataset slug:

`olistbr/brazilian-ecommerce`

Then place the extracted CSV files in:

`data/raw/`

The notebook will use the original file names from Kaggle.

## Claim Boundary

This project uses a public and anonymized dataset for learning, recruitment, and portfolio purposes. The customer segments are exploratory analytical results, not proven business outcomes. Any business recommendations should be treated as decision-support ideas, not validated production decisions.
