# Data Directory

This directory stores dataset files used in the IMV customer segmentation project.

## Dataset Source

Dataset: Olist Brazilian E-Commerce Public Dataset  
Source: <https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce>  
Kaggle slug: `olistbr/brazilian-ecommerce`

## Raw Data

Raw CSV files should be downloaded from Kaggle and placed inside:

`data/raw/`

Recommended terminal command:

```powershell
.\.venv\Scripts\kaggle.exe datasets download -d olistbr/brazilian-ecommerce -p .\data\raw --unzip
```

The raw dataset is not committed to Git because it can be large and should be retrieved from the original source.

## Main Files Planned for Analysis

- `olist_customers_dataset.csv`
- `olist_orders_dataset.csv`
- `olist_order_items_dataset.csv`
- `olist_order_payments_dataset.csv`
- `olist_order_reviews_dataset.csv`
- `olist_products_dataset.csv`

## Optional Files

The following files may be used if they add clear value to the analysis:

- `product_category_name_translation.csv`
- `olist_geolocation_dataset.csv`
- `olist_sellers_dataset.csv`

Optional files should only be included in the analysis when they support the customer segmentation objective. Do not add tables just to make the project look complex.

## Data Usage Note

The repository documents the dataset source, project structure, analysis workflow, and modeling steps. The original Kaggle CSV files stay local in `data/raw/` and are ignored by Git.

To reproduce the project, download the Olist dataset from Kaggle, extract the required CSV files, and place them in `data/raw/` using the original filenames. The notebook will read the files from that local folder.

Do not rename the raw CSV files unless the notebook and documentation are updated consistently.
