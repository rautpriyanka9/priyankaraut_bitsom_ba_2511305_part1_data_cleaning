
# Order Data Cleaning, Validation & Transformation Project

## Project Overview

This project focuses on cleaning, validating, and enriching order transaction data by applying business rules and generating calculated metrics for business reporting and analysis.

The objective is to:

- Improve data quality
- Standardize missing and inconsistent values
- Identify invalid records
- Generate analytical fields
- Create reporting-ready datasets
- Maintain complete auditability of all transformations

#Task 1: Preserve Raw Data

# Dataset

## Input File

```text
cleaned_orders.xlsm
```

## Output File

```text
cleaned_orders_updated.xlsx
```
#Task 2: Clean Text Fields

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

#Task 3: Clean and Validate Dates

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


## Task 4: Handle Duplicates

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