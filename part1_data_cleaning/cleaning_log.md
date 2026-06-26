# Data Cleaning Log

## 1. List of Issues Found
- **Text Formatting Issues:** Several text columns (`segment`, `region`, `category`, etc.) contained leading/trailing spaces, double spaces, and inconsistent capitalization (e.g., `'  North '`, `'NORTH'`).
- **Date Format Inconsistencies:** `order_date` and `ship_date` had mixed string formats (e.g., `21 Jul 2024`, `07/27/2024`, `2025-09-01`).
- **Missing Values:** Found missing values in the `region` and `ship_mode` columns, as well as missing data in `discount`.
- **Invalid Dates:** Found instances where the `ship_date` was earlier than the `order_date`.
- **Duplicate Records:** Found exact duplicate rows and duplicate `order_id` values with conflicting information.
- **Discount Anomalies:** Found cases of discounts that were incorrectly formatted or missing.

## 2. Cleaning Actions Performed
- **Text Cleaning:** Applied stripping, double-space removal, and Title Casing to all text categorical columns using Python's `pandas` string methods.
- **Date Standardization:** Parsed all date strings into standard datetime objects (`YYYY-MM-DD`). 
- **Calculated Shipping Delay:** Created `shipping_delay_days` by subtracting order date from ship date.
- **Handling Duplicates:** Removed exact duplicate rows entirely, keeping only the first occurrence.
- **Flagging Conflicting Duplicates:** Instead of deleting them, duplicate `order_id`s with conflicting info were flagged for review using the `data_quality_flag`.

## 3. Business Rules Applied
- **Missing Region & Ship Mode:** Replaced missing values with `Unknown` and flagged them in the quality report.
- **Missing Discount:** Assumed missing discounts as `0` only if both `quantity` and `unit_price` were valid.
- **Invalid Discount Range:** Flagged discounts greater than 80% (0.8) or negative as invalid.
- **Cancelled/Failed Orders:** Excluded cancelled, returned, refunded, and failed orders from the main pivot summary for sales and profit.
- **Invalid Shipping:** Flagged records where `ship_date` was earlier than `order_date` as invalid.

## 4. Assumptions Made
- Assumed that any discount greater than 1 in the raw data was a percentage (e.g., 20 meant 20%), so it was divided by 100 to get 0.2.
- Assumed the maximum valid discount is 80% (0.8). Anything above was flagged as invalid.
- Assumed that exact duplicate rows were accidental multi-clicks during export and were safe to delete.

## 5. Records Removed
- Removed exact duplicate rows from the dataset (keeping only the first instance). The total number of removed rows is recorded in the Data Quality Report.

## 6. Records Flagged
- **Conflicting Order IDs:** Flagged as `Warning: Conflicting Duplicate Order ID`.
- **Ship Before Order:** Flagged as `Invalid Shipping Record (Ship < Order)`.
- **Missing Region/Ship Mode:** Flagged as `Warning: Missing Region` or `Warning: Missing Ship Mode`.
- **Invalid Discount:** Flagged as `Invalid Discount Range`.
- **Overall Data Quality Flag:** Added a column `data_quality_flag` which marks records as `Clean`, `Warning: [...]`, or `Invalid`.

## 7. Limitations of the Cleaning Process
- We cannot verify which conflicting duplicate order ID is the "true" record without checking the original source database, so they were left in the dataset and just flagged.
- Converting discounts > 1 by dividing by 100 is a heuristic; if a discount was genuinely meant to be a flat currency amount (e.g., $5.00 off), this logic would incorrectly convert it to 5%.
