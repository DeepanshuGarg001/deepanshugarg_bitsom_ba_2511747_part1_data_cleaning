# Business Analytics Capstone: Part 1 - Data Cleaning

## 1. Problem Summary
The company exported order-level sales data from multiple internal systems, which resulted in a messy dataset. There were inconsistent text formatting, date format problems, duplicate records, missing values, invalid discounts, and order status inconsistencies. The objective was to clean, validate, and prepare an analysis-ready version of the data, applying strict business rules and generating summary reports.

## 2. Dataset Description
The dataset used is `raw_orders.xlsx`, which contains 932 rows and 21 columns representing sales transactions. It includes details such as Order ID, Dates, Customer Info, Product Details (Category, Sub-Category), Sales Figures (Quantity, Unit Price, Discount), and Order/Payment Status.

## 3. Tools Used
- **Python**: Used for automated data processing.
- **Pandas**: Used for data manipulation, cleaning, and aggregation.
- **NumPy**: Used for conditional logic and numerical operations.
- **Matplotlib**: Used to generate visual tables for the screenshots.
- **Excel (.xlsx)**: Used as the standard format for reading raw data and outputting the cleaned data and reports.

## 4. Cleaning Steps Performed
- Stripped leading/trailing whitespaces and replaced multiple spaces with a single space across all categorical text columns.
- Converted all text strings to Title Case for consistency.
- Parsed and standardized `order_date` and `ship_date` into `YYYY-MM-DD` format.
- Removed exact duplicate rows.
- Created new calculated columns such as `shipping_delay_days`, `calculated_sales`, and `profit_margin`.

## 5. Business Rules Applied
- Missing `region` and `ship_mode` were filled as `Unknown`.
- Missing `discount` values were set to `0` if sales fields were valid.
- Discounts > 80% (0.8) or negative were flagged as invalid.
- Orders where the ship date occurred before the order date were flagged as invalid.
- Cancelled, Returned, Failed, and Refunded orders were excluded from the main completed sales pivot summaries.

## 6. Summary of Data Quality Issues Found
- Missing data was found in `region` and `ship_mode`.
- The `discount` column contained string formatting errors.
- Some records were exact duplicates, while others shared the same `order_id` but had conflicting information.
- A number of records had invalid shipping dates where the order shipped before it was placed.
- Full details are available in `outputs/data_quality_report.xlsx` and `outputs/cleaning_log.md`.

## 7. Summary of Final Pivot Reports
The pivot summary report includes:
- **Sales and profit by region**: Shows total calculated sales and profit per region, sorted highest to lowest.
- **Sales and profit by category**: A breakdown of performance by category and sub-category.
- **Order count by ship mode**: Frequency of different shipping methods.
- **Profit margin by customer segment**: Comparing profitability across Consumer, Corporate, etc.
- **Bad Orders by Region**: A summary of Refunded, Cancelled, and Failed orders.
- **Monthly sales trend**: Sales grouped by Year and Month.

## 8. Key Business Insights
1. **Sales by Region**: The West and East regions consistently bring in the most sales, whereas the South region trails behind and may need targeted marketing campaigns.
2. **Profit Margins**: The "Consumer" segment drives the highest volume, but checking the profit margins across segments shows that B2B (Corporate/Small Business) segments often yield higher per-unit margins depending on the discounts offered.
3. **Shipping Delays**: First Class and Same Day shipping perform well, but a significant number of orders have invalid date sequencing which suggests a flaw in the warehouse scanning system.

## 9. Assumptions and Limitations
- It was assumed that a discount listed as a number greater than 1 (e.g. 20) meant 20%, so it was converted to 0.2.
- Conflicting duplicate order IDs were kept in the dataset and flagged, because without the source system, we cannot know which row is the correct one.
- Data conversion heuristics might fail if future data introduces entirely new date formats.

## 10. Screenshots

### Raw Data Preview
![Raw Data Preview](screenshots/raw_data_preview.png)

### Cleaned Data Preview
![Cleaned Data Preview](screenshots/cleaned_data_preview.png)

### Pivot Summary 1: Sales and Profit by Region
![Sales and Profit by Region](screenshots/pivot_summary_1.png)

### Pivot Summary 2: Order Count by Ship Mode
![Order Count by Ship Mode](screenshots/pivot_summary_2.png)
