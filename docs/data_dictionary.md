# Data Dictionary Draft

This file documents the main Olist tables planned for the project.

## `olist_customers_dataset.csv`

Customer identity and location information.

Important columns:

- `customer_id`
- `customer_unique_id`
- `customer_zip_code_prefix`
- `customer_city`
- `customer_state`

## `olist_orders_dataset.csv`

Order status and timestamp information.

Important columns:

- `order_id`
- `customer_id`
- `order_status`
- `order_purchase_timestamp`
- `order_approved_at`
- `order_delivered_carrier_date`
- `order_delivered_customer_date`
- `order_estimated_delivery_date`

## `olist_order_items_dataset.csv`

Order item, price, freight, product, and seller information.

Important columns:

- `order_id`
- `order_item_id`
- `product_id`
- `seller_id`
- `price`
- `freight_value`

## `olist_order_payments_dataset.csv`

Payment method and payment value information.

Important columns:

- `order_id`
- `payment_sequential`
- `payment_type`
- `payment_installments`
- `payment_value`

## `olist_order_reviews_dataset.csv`

Customer review information.

Important columns:

- `review_id`
- `order_id`
- `review_score`
- `review_creation_date`
- `review_answer_timestamp`

## `olist_products_dataset.csv`

Product category and product attributes.

Important columns:

- `product_id`
- `product_category_name`
- `product_name_lenght`
- `product_description_lenght`
- `product_photos_qty`
- `product_weight_g`
- `product_length_cm`
- `product_height_cm`
- `product_width_cm`

## Notes

This dictionary will be refined after the first data inspection.
