# Data Cleaning Log

## Project Overview

This document records all data quality issues identified, cleaning actions performed, business rules applied, assumptions made, records removed or flagged, and limitations encountered during the data cleaning and validation process.

---

# Dataset Information

**Source File**

```text
cleaned_orders.xlsm
```

**Processed File**

```text
cleaned_orders_updated.xlsx
```

---

# Issues Identified

The following data quality issues were identified during assessment:

## Missing Values

- Missing region values
- Missing ship mode values
- Missing discount values
- Missing order dates
- Missing ship dates

## Date Issues

- Invalid date formats
- Text values stored in date fields
- Ship dates occurring before order dates

## Duplicate Issues

- Exact duplicate rows
- Duplicate order IDs
- Duplicate order IDs containing conflicting information

## Text Standardization Issues

- Leading and trailing spaces
- Multiple internal spaces
- Inconsistent capitalization
- Unwanted special characters
- Inconsistent category and sub-category naming

## Business Rule Violations

- Negative discount values
- Discounts exceeding allowed business limits
- Cancelled orders included in sales data
- Failed payment transactions included in sales data
- Refunded orders mixed with completed sales records

---

# Cleaning Actions Performed

## Text Cleaning

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

### Actions Performed

- Removed leading and trailing spaces using TRIM
- Removed extra internal spaces
- Standardized capitalization using PROPER
- Removed unwanted special characters using SUBSTITUTE
- Consolidated inconsistent category names
- Consolidated inconsistent sub-category names
- Applied Find and Replace where necessary

---

## Date Cleaning

### Fields Processed

- order_date
- ship_date

### Actions Performed

- Converted dates into a consistent YYYY-MM-DD format
- Identified missing dates
- Identified invalid date values
- Identified text values not recognized as dates
- Flagged ship dates occurring before order dates
- Calculated shipping_delay_days

---

## Duplicate Handling

### Actions Performed

- Identified exact duplicate rows
- Identified duplicate order IDs
- Identified duplicate order IDs containing conflicting information

### Handling Approach

| Duplicate Type | Action |
|---------------|---------|
| Exact Duplicate | Removed |
| Duplicate Order ID with Same Data | Retained Single Record |
| Duplicate Order ID with Conflicting Data | Flagged for Review |

Conflicting duplicate records were not deleted automatically.

---

## Calculated Columns Created

The following calculated columns were added:

- cleaned_discount
- calculated_sales
- calculated_profit
- profit_margin
- shipping_delay_days
- order_month
- order_year
- data_quality_flag

---

# Business Rules Applied

## Missing Region

**Rule**

Replace missing values with `Unknown`

**Action**

- Missing region values replaced with `Unknown`
- Records flagged for audit purposes

---

## Missing Ship Mode

**Rule**

Replace missing values with `Unknown`

**Action**

- Missing ship mode values replaced with `Unknown`
- Records flagged for audit purposes

---

## Missing Discount

**Rule**

Missing discounts treated as 0 only when other sales-related fields were valid

**Action**

- Missing discount values standardized to 0 where appropriate

---

## Negative Discount

**Rule**

Negative discount values are invalid

**Action**

- Flagged as invalid records

---

## Discount Above Allowed Range

**Rule**

Discounts exceeding approved business limits are invalid

**Action**

- Flagged as invalid records

---

## Cancelled Orders

**Rule**

Cancelled orders excluded from completed sales reporting

**Action**

- Excluded from completed sales summaries

---

## Failed Payments

**Rule**

Failed payment records excluded from completed sales reporting

**Action**

- Excluded from completed sales summaries

---

## Refunded Orders

**Rule**

Refunded orders must be reported separately

**Action**

- Included in refund-specific summaries

---

## Invalid Shipping Dates

**Rule**

Ship date cannot occur before order date

**Action**

- Flagged as invalid shipping records

---

# Assumptions Made

The following assumptions were applied during cleaning:

1. Blank region values represent unknown regions.
2. Blank ship mode values represent unknown shipping methods.
3. Missing discounts can be treated as 0 only when all other sales-related fields are valid.
4. Date values must be recognized by Excel as valid dates.
5. Duplicate records with conflicting information require manual review and should not be removed automatically.
6. Profit calculations are based on calculated sales minus cost.
7. Profit margin is calculated as profit divided by sales.
8. Refunded orders remain valid records but are excluded from completed sales reporting.

---

# Records Removed

### Records Removed from Dataset

- Exact duplicate records only

### Removal Policy

- One copy retained
- Additional exact duplicates removed

---

# Records Flagged

The following records were flagged for review:

- Duplicate order IDs with conflicting information
- Negative discount values
- Discounts exceeding allowed limits
- Invalid shipping dates
- Missing critical date fields
- Invalid date formats
- Text values stored in date fields

Flagged records were retained for auditability and manual review.

---

# Limitations

The following limitations apply to the cleaning process:

1. Duplicate records with conflicting values require business-user review before final resolution.
2. Some category standardization decisions may require domain-specific validation.
3. Date validation depends on values being interpretable by Excel.
4. Business-specific discount limits were applied based on available requirements and may require confirmation.
5. Data quality checks cannot verify external business events or source-system accuracy.
6. Automated cleaning cannot determine the correct value when conflicting information exists across duplicate records.

---

# Deliverables Generated

- cleaned_orders_updated.xlsx
- data_quality_report.xlsx
- pivot_summary.xlsx
- cleaning_log.md

---

# Conclusion

The dataset was successfully cleaned, standardized, validated, and enriched using defined business rules and data quality checks. All transformations, assumptions, flagged records, and removed records have been documented to ensure transparency, auditability, and reporting readiness.