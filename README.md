# IMV Customer Segmentation with K-Means

Customer segmentation project using K-Means Clustering on the Olist Brazilian E-Commerce Public Dataset. This repository is prepared as an individual Machine Learning assignment for IMV Laboratory Recruitment 2026.

## Project Status

This project has completed the main modeling workflow and the HackMD coding documentation draft.

The project foundation, dataset acquisition, raw data inspection, customer-level feature engineering, preprocessing, K-Means model evaluation, segment interpretation, report tables, visualization outputs, and HackMD documentation draft have been prepared. The current modeling result produces a four-segment customer clustering solution supported by clustering metrics, business interpretation, documentation tables, and visualization reports.

The remaining private submission tasks are final HackMD publishing, final presentation slides, video presentation recording, and final submission links.

## Assignment Context

| Item                  | Description                                                               |
| --------------------- | ------------------------------------------------------------------------- |
| Assignment            | Individual Machine Learning Task                                          |
| Recruitment           | IMV Laboratory Recruitment 2026                                           |
| Topic                 | Customer Segmentation Based on Shopping Behavior Using K-Means Clustering |
| Machine Learning Type | Unsupervised Learning                                                     |
| Required Method       | K-Means Clustering                                                        |
| Author                | Muhammad Zaenal Abidin Abdurrahman                                        |
| Student ID            | 101012300153                                                              |

## Project Overview

This project aims to group e-commerce customers into meaningful behavioral segments based on their shopping patterns. The analysis uses transaction, payment, review, delivery, and product information from the Olist Brazilian E-Commerce Public Dataset.

The main idea is to transform raw relational e-commerce tables into a customer-level dataset, then use K-Means Clustering to identify groups of customers with similar behavior.

The project focuses on:

- understanding the raw Olist dataset structure,
- checking data quality before modeling,
- joining relevant e-commerce tables carefully,
- building customer-level behavioral features,
- validating missing values, duplicates, and feature distributions,
- scaling numerical features for distance-based clustering,
- evaluating several K-Means cluster counts,
- selecting the final number of clusters based on metrics and interpretability,
- explaining each customer segment in business-friendly language.

## Business Objective

E-commerce businesses often need to understand that not all customers behave the same way. Some customers buy once with low spending, some buy higher-value products through installment payments, some experience delivery delays and leave poor reviews, and a smaller group may show repeat purchase behavior.

The objective of this project is to create customer segments that can support practical business analysis, such as:

- identifying low-value one-time customers,
- understanding higher-value installment customers,
- detecting customers affected by delivery problems,
- recognizing repeat customers with broader baskets,
- preparing better customer retention and service improvement strategies.

This is an unsupervised learning project, so there is no target label and no accuracy score. The result is evaluated using clustering metrics and business interpretability.

## Dataset

Dataset: Olist Brazilian E-Commerce Public Dataset  
Source: <https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce>

The Olist dataset contains anonymized Brazilian e-commerce orders from multiple relational CSV files. The raw tables include order information, customer identifiers, payment behavior, product details, review scores, seller information, product category translation, and geolocation data.

For this project, the modeling dataset is built at the `customer_unique_id` level. This level is used because the goal is customer segmentation, not order segmentation.

Main raw files used in the current modeling workflow:

| File                               | Role in This Project                                                                         |
| ---------------------------------- | -------------------------------------------------------------------------------------------- |
| `olist_customers_dataset.csv`      | Connects order-level customer IDs to unique customer IDs and customer location information.  |
| `olist_orders_dataset.csv`         | Provides order status and order lifecycle timestamps used for recency and delivery behavior. |
| `olist_order_items_dataset.csv`    | Provides item-level order information, price, freight value, product ID, and seller ID.      |
| `olist_order_payments_dataset.csv` | Provides payment type, installment count, and payment value.                                 |
| `olist_order_reviews_dataset.csv`  | Provides review score and review timestamps for customer satisfaction signals.               |
| `olist_products_dataset.csv`       | Provides product metadata and product category information.                                  |

Files not used directly in the current clustering feature matrix:

| File                                    | Reason                                                                                                                                                                                                          |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `olist_geolocation_dataset.csv`         | Useful for advanced geospatial analysis, but not required for the current customer behavior segmentation checkpoint. It is intentionally excluded to keep the first modeling version focused and interpretable. |
| `olist_sellers_dataset.csv`             | More relevant for seller analysis than customer segmentation. Seller information can be added later if the project expands into marketplace-side analysis.                                                      |
| `product_category_name_translation.csv` | Useful for readable category names, but the current model uses product diversity/count behavior rather than category-level content analysis.                                                                    |

## Methodology

The project follows a structured notebook-based workflow.

1. Raw data inspection  
   The first notebook checks dataset availability, table shapes, column names, missing values, duplicate rows, and basic table relationships.

2. Customer-level feature engineering  
   The second notebook joins relevant Olist tables and aggregates transaction records into customer-level behavioral features.

3. Feature validation and preprocessing  
   The third notebook validates missing values, checks modeling columns, handles feature readiness, and applies scaling for K-Means.

4. K-Means modeling and cluster interpretation  
   The fourth notebook compares several cluster counts, evaluates the clustering result, selects the final `K`, assigns customer segments, and prepares business interpretation.

## Current Modeling Result

The current K-Means solution uses `K=4`.

This selection is based on a balance between:

- inertia reduction from the elbow curve,
- silhouette score,
- Davies-Bouldin score,
- cluster size balance,
- business interpretability,
- whether the resulting segments can be explained clearly without forcing an overly complex model.

The final customer segments are:

| Segment                                               | Customer Share | Main Interpretation                                                            |
| ----------------------------------------------------- | -------------: | ------------------------------------------------------------------------------ |
| Low-value one-time customers with high freight burden |         45.11% | One-time buyers with low payment value and relatively higher shipping burden.  |
| Higher-value installment one-time customers           |         43.62% | One-time buyers with higher payment value and higher installment usage.        |
| Delayed and dissatisfied customers                    |          7.93% | Customers with lower review scores, longer delivery time, and late deliveries. |
| Repeat and broader-basket customers                   |          3.34% | Repeat buyers with higher payment value and more product variety.              |

## Evaluation Summary

Because this is an unsupervised learning project, the evaluation does not use accuracy, precision, recall, or F1-score. Those metrics require true labels, while this project does not have predefined customer segment labels.

The current evaluation uses:

- inertia for within-cluster compactness,
- elbow method for cluster count comparison,
- silhouette score for separation and cohesion,
- Davies-Bouldin score for cluster separation quality,
- cluster size distribution to avoid impractical segmentation,
- business interpretation to make sure the segments are understandable and useful.

## Repository Structure

```text
.
├── data/
│   ├── external/
│   ├── interim/
│   ├── processed/
│   └── raw/
├── docs/
│   ├── dataset_source.md
│   ├── data_dictionary.md
│   ├── raw_data_audit.md
│   ├── feature_engineering_summary.md
│   ├── preprocessing_summary.md
│   ├── kmeans_evaluation_summary.md
│   ├── hackmd_documentation_draft.md
│   └── project_notes.md
├── notebooks/
│   ├── 01_raw_data_inspection.ipynb
│   ├── 02_customer_feature_engineering.ipynb
│   ├── 03_feature_validation_preprocessing.ipynb
│   └── 04_kmeans_modeling_and_cluster_interpretation.ipynb
├── reports/
│   ├── figures/
│   └── tables/
├── .gitignore
├── README.md
└── requirements.txt
```

Notes:

- Raw and processed datasets are not committed to Git.
- Generated report figures and small Markdown summary tables are committed because they help reviewers understand the modeling result.
- Local files such as `kaggle.json`, virtual environments, caches, and notebook checkpoints are ignored.

## Local Setup

Clone the repository:

```powershell
git clone https://github.com/Zendin110206/imv-customer-segmentation-kmeans.git
cd imv-customer-segmentation-kmeans
```

Create and activate a virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Upgrade `pip` and install dependencies:

```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

## Dataset Setup

The dataset is downloaded from Kaggle:

```text
https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce
```

Place the Olist CSV files inside:

```text
data/raw/
```

Expected raw CSV files:

```text
data/raw/olist_customers_dataset.csv
data/raw/olist_geolocation_dataset.csv
data/raw/olist_order_items_dataset.csv
data/raw/olist_order_payments_dataset.csv
data/raw/olist_order_reviews_dataset.csv
data/raw/olist_orders_dataset.csv
data/raw/olist_products_dataset.csv
data/raw/olist_sellers_dataset.csv
data/raw/product_category_name_translation.csv
```

If Kaggle CLI is configured, the dataset can also be downloaded with:

```powershell
kaggle datasets download -d olistbr/brazilian-ecommerce -p data/raw --unzip
```

## How to Run

Run the notebooks in this order:

1. `notebooks/01_raw_data_inspection.ipynb`
2. `notebooks/02_customer_feature_engineering.ipynb`
3. `notebooks/03_feature_validation_preprocessing.ipynb`
4. `notebooks/04_kmeans_modeling_and_cluster_interpretation.ipynb`

The notebooks are intentionally ordered because each stage produces outputs used by the next stage.

Optional terminal execution for validation:

```powershell
.\.venv\Scripts\python -m jupyter nbconvert --to notebook --execute notebooks/01_raw_data_inspection.ipynb --output 01_raw_data_inspection.executed.ipynb --output-dir reports/executed_notebooks
.\.venv\Scripts\python -m jupyter nbconvert --to notebook --execute notebooks/02_customer_feature_engineering.ipynb --output 02_customer_feature_engineering.executed.ipynb --output-dir reports/executed_notebooks
.\.venv\Scripts\python -m jupyter nbconvert --to notebook --execute notebooks/03_feature_validation_preprocessing.ipynb --output 03_feature_validation_preprocessing.executed.ipynb --output-dir reports/executed_notebooks
.\.venv\Scripts\python -m jupyter nbconvert --to notebook --execute notebooks/04_kmeans_modeling_and_cluster_interpretation.ipynb --output 04_kmeans_modeling_and_cluster_interpretation.executed.ipynb --output-dir reports/executed_notebooks
```

The executed notebook copies in `reports/executed_notebooks/` are validation artifacts and do not need to be committed.

## Reports and Documentation

Current project documentation:

- [Dataset Source](docs/dataset_source.md)
- [Data Dictionary](docs/data_dictionary.md)
- [Raw Data Audit](docs/raw_data_audit.md)
- [Feature Engineering Summary](docs/feature_engineering_summary.md)
- [Preprocessing Summary](docs/preprocessing_summary.md)
- [K-Means Evaluation Summary](docs/kmeans_evaluation_summary.md)
- [HackMD Documentation Draft](docs/hackmd_documentation_draft.md)
- [Project Notes](docs/project_notes.md)
- [K-Means Evaluation Metrics Table](reports/tables/kmeans_evaluation_metrics.md)
- [Customer Segment Profiles Table](reports/tables/customer_segment_profiles.md)
- [Segment Interpretation Table](reports/tables/segment_interpretation.md)

Current generated figures:

- `reports/figures/kmeans_elbow_curve.png`
- `reports/figures/kmeans_metric_comparison.png`
- `reports/figures/kmeans_cluster_balance.png`
- `reports/figures/customer_segment_size_distribution.png`
- `reports/figures/customer_segment_business_kpi_summary.png`
- `reports/figures/customer_segments_pca.png`
- `reports/figures/customer_segments_pca_3d.png`
- `reports/figures/customer_segment_profile_heatmap.png`

The HackMD documentation draft is prepared in `docs/hackmd_documentation_draft.md`. Final presentation slides and video files are intentionally kept outside the public repository until they are finalized for submission.

## Deliverables

Required IMV assignment deliverables:

- Jupyter Notebook file (`.ipynb`)
- HackMD coding documentation
- Presentation slides (`.pptx`)
- Video presentation

Current repository checkpoint:

- notebook-based data inspection is prepared,
- customer-level feature engineering is prepared,
- preprocessing and feature validation are prepared,
- K-Means model evaluation and segment interpretation are prepared,
- visual report figures are prepared,
- Markdown summary tables are prepared,
- HackMD coding documentation draft is prepared.

## Important Limitations

This project uses public anonymized e-commerce data, not internal company data. Therefore, the segments should be interpreted as analytical learning results, not production customer labels.

The current clustering result is based on behavior available in the Olist dataset, such as order frequency, payment value, delivery timing, review score, freight burden, and product diversity. It does not include demographic information, marketing campaign exposure, browsing behavior, or customer lifetime data outside the dataset period.

The geolocation table is not used in the current model to keep the first clustering version focused on customer shopping behavior. It can be explored later if the project expands into regional segmentation or delivery-distance analysis.

## Author

Muhammad Zaenal Abidin Abdurrahman  
GitHub: [Zendin110206](https://github.com/Zendin110206)
