# Sales Orders Data Cleaning and Analysis Project

## Problem Summary

The objective of this project was to clean, validate, standardize, and analyze a sales orders dataset containing customer, product, shipping, sales, and order status information.

The dataset contained various data quality issues including:

- Missing values
- Invalid dates
- Duplicate records
- Inconsistent text formatting
- Invalid discount values
- Business rule violations
- Order status inconsistencies

The goal was to improve data quality, apply business rules, create validation reports, and generate business insights through pivot analysis.

---

# Dataset Description

## Source Dataset

The dataset contains transactional sales order data with information related to:

- Order ID
- Customer Name
- Customer Segment
- Region
- State
- City
- Category
- Sub-Category
- Ship Mode
- Order Date
- Ship Date
- Quantity
- Unit Price
- Discount
- Sales
- Cost
- Profit
- Payment Status
- Order Status

![Raw Dataset Preview](screenshots/raw_data_preview.png)

## Dataset Size

| Metric | Value |
|---------|---------|
| Total Records | 933 |
| Total Columns | 21 |
| Data Range | Columns A to U |

---

# Tools Used

The following tools and techniques were used:

## Microsoft Excel

- Data Cleaning
- Data Validation
- Pivot Tables
- Conditional Formatting
- Filters and Sorting

## Excel Functions

- TRIM()
- PROPER()
- SUBSTITUTE()
- IF()
- IFERROR()
- COUNTIF()
- COUNTIFS()
- SUMIFS()
- XLOOKUP()
- DATEVALUE()
- YEAR()
- MONTH()
- DATEDIF()
- TEXTJOIN()

---

# Cleaning Steps Performed

# Task 1 – Preserve Raw Data

# Dataset

## Input File

```text
cleaned_orders.xlsm
```

## Output File

```text
cleaned_orders_updated.xlsx
```
# Task 2 – Clean Text Fields

### Text Standardization Performed

The following fields were standardized:

- customer_name
- segment
- region
- state
- city
- category
- sub_category
- ship_mode
- payment_status
- order_status

Cleaning techniques used:

- TRIM() to remove leading/trailing spaces
- SUBSTITUTE() to remove unwanted special characters
- PROPER() to standardize capitalization
- Find & Replace to consolidate inconsistent values
- Flash Fill to normalize customer names
- Manual validation using filters and unique value checks

Result:
All text fields were standardized into consistent business-friendly formats suitable for reporting and analysis.

# Task 3 – Clean and Validate Dates

### Date Cleaning and Validation

Fields Processed:
- order_date
- ship_date

Actions Performed:
- Converted all valid dates to YYYY-MM-DD format.
- Identified and flagged missing dates.
- Identified invalid date values and text values not recognized as dates.
- Flagged records where ship_date occurred before order_date.
- Calculated shipping_delay_days as the difference between ship_date and order_date.

Validation Rules:
- Dates must be recognized by Excel as valid date values.
- Ship dates cannot occur before order dates.
- Missing or invalid dates are marked as invalid records.

### Date Cleaning and Validation

Fields Processed:
- order_date
- ship_date

Actions Performed:
- Converted all valid dates to YYYY-MM-DD format.
- Identified and flagged missing dates.
- Identified invalid date values and text values not recognized as dates.
- Flagged records where ship_date occurred before order_date.
- Calculated shipping_delay_days as the difference between ship_date and order_date.

Validation Rules:
- Dates must be recognized by Excel as valid date values.
- Ship dates cannot occur before order dates.
- Missing or invalid dates are marked as invalid records.


# Task 4 – Handle Duplicates

### Exact Duplicate Rows

- Exact duplicate records were identified by comparing all available columns.
- Duplicate copies were marked for removal.
- One instance of each record was retained.

### Duplicate Order IDs

- Duplicate order IDs were analyzed separately.
- Records with identical values were treated as exact duplicates.
- Records with conflicting values were flagged for review.

### Handling Policy

| Duplicate Type | Action |
|---------------|---------|
| Exact Duplicate | Removed |
| Duplicate Order ID with Same Data | Retained Single Record |
| Duplicate Order ID with Conflicting Data | Flagged for Review |

Conflicting duplicate records were never removed automatically to preserve data integrity and ensure auditability.

# Task 5 – Business Rule Implementation

The following business rules were applied to the dataset.

## Rule 1: Missing Region

### Requirement

Missing region values must be replaced with "Unknown" and flagged.

### Action Performed

- Replaced blank/null region values with `Unknown`
- Added quality flag for affected records

---

## Rule 2: Missing Ship Mode

### Requirement

Missing ship mode values must be replaced with "Unknown" and flagged.

### Action Performed

- Replaced blank/null ship mode values with `Unknown`
- Added quality flag for affected records

---

## Rule 3: Missing Discount

### Requirement

Missing discount values should be treated as 0 only if all other sales-related fields are valid.

### Action Performed

- Validated sales-related fields
- Replaced eligible missing discount values with `0`
- Flagged affected records

---

## Rule 4: Negative Discount

### Requirement

Negative discount values are invalid.

### Action Performed

- Identified records with discount less than 0
- Marked records as invalid

---

## Rule 5: Discount Above Allowed Range

### Requirement

Discount values exceeding the allowable range are invalid.

### Action Performed

- Identified records where discount exceeded the acceptable threshold
- Marked records as invalid

---

## Rule 6: Cancelled Orders

### Requirement

Cancelled orders should not contribute to completed sales reporting.

### Action Performed

- Excluded cancelled orders from completed sales summaries

---

## Rule 7: Failed Payments

### Requirement

Failed payment transactions should not contribute to completed sales reporting.

### Action Performed

- Excluded failed payment records from completed sales summaries

---

## Rule 8: Refunded Orders

### Requirement

Refunded orders must be summarized separately.

### Action Performed

- Created a dedicated refund summary

---

## Rule 9: Invalid Shipping Dates

### Requirement

Ship date occurring before order date is invalid.

### Action Performed

- Identified shipping date anomalies
- Marked records as invalid

---

# Task 6 – Calculated Columns

Additional calculated fields were created to support analysis and reporting.

## 1. cleaned_discount

### Purpose

Standardizes discount values.

### Logic

```text
If discount is blank → 0
Else → original discount
```

---

## 2. calculated_sales

### Purpose

Calculates actual sales after discount.

### Formula

```text
quantity × unit_price × (1 − cleaned_discount)
```

---

## 3. calculated_profit

### Purpose

Calculates profit earned from each order.

### Formula

```text
calculated_sales − cost
```

---

## 4. profit_margin

### Purpose

Measures profitability percentage.

### Formula

```text
calculated_profit ÷ calculated_sales
```

---

## 5. shipping_delay_days

### Purpose

Measures shipping turnaround time.

### Formula

```text
ship_date − order_date
```

---

## 6. order_month

### Purpose

Extracts month from order date.

### Example

```text
2024-07-21 → July
```

---

## 7. order_year

### Purpose

Extracts year from order date.

### Example

```text
2024-07-21 → 2024
```

---

## 8. data_quality_flag

### Purpose

Provides an overall quality assessment for each record.

### Classification Rules

| Condition | Flag |
|------------|--------|
| No issues found | Clean |
| Missing optional fields | Warning |
| Invalid discount or shipping dates | Invalid |




## Completed Sales Summary

Includes:

- Successful orders
- Non-cancelled orders
- Non-refunded orders

Metrics Generated:

- Total Orders
- Total Sales
- Total Cost
- Total Profit

---

## Refunded Orders Summary

Includes:

- Refunded Order Count
- Refunded Sales
- Refunded Cost
- Refunded Profit

---

# Documentation

All business rule decisions, assumptions, validation checks, and data-cleaning activities are documented in:

```text
outputs/cleaning_log.md
```

---

# Deliverables

- ✅ Cleaned Dataset
- ✅ Business Rule Validation
- ✅ Calculated Columns
- ✅ Data Quality Flags
- ✅ Completed Sales Summary
- ✅ Refunded Orders Summary
- ✅ Cleaning Log
- ✅ README Documentation

---

# Conclusion

The dataset was successfully cleaned, validated, and enriched using predefined business rules and calculated metrics. The resulting dataset is reporting-ready, auditable, and suitable for business analysis and decision-making.

# Task 7 – Data Quality Report

## Objective

A comprehensive data quality assessment report was created to summarize data issues identified during the cleaning and validation process. The report provides an evaluator-friendly overview of data quality metrics, validation results, duplicate analysis, and record quality classification.

---

## Output File

```text
outputs/data_quality_report.xlsx
```

---

## Report Structure

The workbook contains multiple worksheets, each focusing on a specific aspect of data quality.

### 1. Missing Value Summary

**Purpose:**

- Identify missing values across critical fields.

**Metrics Included:**

- Missing Region Count
- Missing Ship Mode Count
- Missing Discount Count
- Missing Order Date Count
- Missing Ship Date Count
- Missing Sales/Cost Values

**Outcome:**

- Missing values were quantified and documented for further analysis.

---

### 2. Duplicate Summary

**Purpose:**

- Identify exact duplicate records.
- Detect duplicate Order IDs.
- Highlight conflicting duplicate records requiring manual review.

**Metrics Included:**

| Metric | Description |
|----------|-------------|
| Exact Duplicate Rows Found | Number of fully duplicated records |
| Duplicate Order IDs Found | Number of repeated Order IDs |
| Records Removed | Exact duplicate records removed |
| Records Flagged for Review | Conflicting duplicate records |
| Handling Logic | Duplicate resolution approach |

#### Duplicate Handling Policy

| Duplicate Type | Action |
|---------------|---------|
| Exact Duplicate | Removed |
| Duplicate Order ID with Same Data | Retained Single Record |
| Duplicate Order ID with Conflicting Data | Flagged for Review |

No conflicting duplicate records were deleted automatically.

---

### 3. Invalid Discount Summary

**Purpose:**

- Identify discount-related data quality issues.

**Metrics Included:**

- Missing Discount Values
- Negative Discounts
- Discounts Above Allowed Range

**Validation Rules:**

- Discount must be greater than or equal to 0.
- Discount must remain within the approved business range.

---

### 4. Date Issue Summary

**Purpose:**

- Identify invalid or inconsistent date values.

**Metrics Included:**

- Missing Order Dates
- Missing Ship Dates
- Invalid Date Formats
- Ship Date Earlier Than Order Date

**Validation Rules:**

- Dates must be valid Excel-recognized dates.
- Ship dates cannot occur before order dates.

---

### 5. Order Status Issue Summary

**Purpose:**

- Review order and payment status distributions.

**Metrics Included:**

- Cancelled Orders
- Failed Payments
- Refunded Orders
- Completed Orders

**Business Rules Applied:**

- Cancelled orders excluded from completed sales reporting.
- Failed payments excluded from completed sales reporting.
- Refunded orders reported separately.

---

### 6. Sales and Profit Calculation Mismatch Summary

**Purpose:**

- Validate calculated sales and profit values.

**Checks Performed:**

- Original Sales vs Calculated Sales
- Original Profit vs Calculated Profit

**Metrics Included:**

- Sales Mismatch Count
- Profit Mismatch Count

Records exceeding the accepted variance threshold were flagged for review.

---

### 7. Clean vs Flagged Record Count

**Purpose:**

- Provide an overall data quality classification.

**Categories:**

| Status | Description |
|----------|-------------|
| Clean | No issues identified |
| Warning | Minor data quality issues detected |
| Invalid | Business rule violation detected |

**Metrics Included:**

- Total Clean Records
- Total Warning Records
- Total Invalid Records

---

## Quality Assessment Methodology

The report was generated using the following validation techniques:

- Missing value analysis
- Duplicate detection
- Business rule validation
- Date consistency checks
- Discount validation checks
- Sales and profit recalculation
- Record quality classification

---

## Deliverables

- ✅ Missing Value Summary
- ✅ Duplicate Summary
- ✅ Invalid Discount Summary
- ✅ Date Issue Summary
- ✅ Order Status Issue Summary
- ✅ Sales/Profit Calculation Mismatch Summary
- ✅ Clean vs Flagged Record Count

---

## Conclusion

The Data Quality Report provides a complete assessment of dataset integrity and compliance with defined business rules. All identified issues were documented, flagged, and summarized to ensure transparency, auditability, and reporting readiness.

## Task 8 – Pivot Summary Report

A pivot-based reporting workbook was created to provide business insights from the cleaned dataset.

### Output File

```text
outputs/pivot_summary.xlsx
```

### Pivot Reports Included

1. Sales and Profit by Region
2. Sales and Profit by Category and Sub-Category
3. Order Count by Ship Mode
4. Profit Margin by Customer Segment
5. Refunded, Cancelled, and Failed Orders by Region
6. Monthly Sales Trend

### Sorting and Filtering Applied

- Category and Sub-Category Sales Summary sorted by Total Sales (descending).
- Order Status by Region pivot includes Region filtering.

### Purpose

The pivot summaries provide a high-level business view of revenue, profitability, customer segments, shipping methods, order performance, and sales trends over time.