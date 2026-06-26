# Order Data Cleaning & Validation Project

## Overview

This project performs data cleaning, validation, and business-rule enforcement on the provided order dataset. The objective is to improve data quality, identify invalid records, and generate accurate business summaries for completed and refunded orders.

---

# Files Included

| File | Description |
|--------|-------------|
| cleaned_orders.xlsm | Original cleaned dataset provided for processing |
| cleaned_orders_updated.xlsx | Dataset after applying business rules and quality flags |
| outputs/cleaning_log.md | Detailed documentation of all cleaning decisions and business-rule applications |
| README.md | Project documentation and summary |

---

# Business Rules Applied

## 1. Missing Region

**Rule:** Fill missing region values with `Unknown` and flag the record.

### Action Taken
- Replaced blank/null region values with `Unknown`
- Added quality flag: `flag_missing_region`

---

## 2. Missing Ship Mode

**Rule:** Fill missing ship_mode values with `Unknown` and flag the record.

### Action Taken
- Replaced blank/null ship_mode values with `Unknown`
- Added quality flag: `flag_missing_ship_mode`

---

## 3. Missing Discount

**Rule:** Treat missing discount as `0` only when all other sales-related fields are valid.

### Action Taken
- Validated sales-related fields
- Replaced eligible missing discount values with `0`
- Added quality flag: `flag_missing_discount`

---

## 4. Negative Discount

**Rule:** Negative discounts are invalid.

### Action Taken
- Identified records where discount < 0
- Added quality flag: `flag_negative_discount`

---

## 5. Discount Above Allowed Range

**Rule:** Discounts above the allowed range are invalid.

### Action Taken
- Identified records where discount exceeded the accepted threshold
- Added quality flag: `flag_discount_out_of_range`

---

## 6. Cancelled Orders

**Rule:** Cancelled orders should not contribute to completed sales reporting.

### Action Taken
- Excluded cancelled orders from completed sales summary

---

## 7. Failed Payments

**Rule:** Failed payments should not contribute to completed sales reporting.

### Action Taken
- Excluded failed payment records from completed sales summary

---

## 8. Refunded Orders

**Rule:** Refunded orders must be reported separately.

### Action Taken
- Created a dedicated refunded orders summary sheet

---

## 9. Invalid Shipping Dates

**Rule:** Ship date occurring before order date is invalid.

### Action Taken
- Identified invalid shipping records
- Added quality flag: `flag_invalid_shipping`

---

# Quality Flags Generated

The following columns were added to support auditability:

| Flag Column | Description |
|------------|-------------|
| flag_missing_region | Region was missing |
| flag_missing_ship_mode | Ship mode was missing |
| flag_missing_discount | Discount was missing |
| flag_negative_discount | Negative discount detected |
| flag_discount_out_of_range | Discount exceeds allowed range |
| flag_invalid_shipping | Ship date occurs before order date |
| any_rule_flag | Record triggered one or more quality rules |

---

# Summary Outputs

## Completed Sales Summary

Includes only:
- Successful orders
- Non-cancelled orders
- Non-refunded orders

Excludes:
- Cancelled orders
- Failed payments
- Refunded orders

### Metrics Generated
- Total Orders
- Total Sales
- Total Cost
- Total Profit

---

## Refunded Orders Summary

Provides separate reporting for:
- Refunded order count
- Refunded sales value
- Refunded cost value
- Refunded profit value

---

# Auditability

All cleaning activities, assumptions, validation checks, and business-rule decisions are documented in:

```text
outputs/cleaning_log.md
```

This ensures complete traceability and reproducibility of the data-cleaning process.

---

# Assumptions

1. Missing `region` and `ship_mode` values are replaced with `Unknown`.
2. Missing `discount` values are converted to `0` only when other sales fields are valid.
3. Discount values are expected to remain within the approved business range.
4. Ship dates must never precede order dates.
5. Cancelled and failed-payment orders are excluded from completed sales reporting.
6. Refunded transactions are reported independently.

---

# Deliverables

- ✅ Cleaned Dataset
- ✅ Business Rule Validation
- ✅ Quality Flags
- ✅ Completed Sales Summary
- ✅ Refunded Sales Summary
- ✅ Cleaning Log Documentation
- ✅ Project README

---

## Author Notes

This project was completed by applying predefined business rules, validating data quality issues, generating audit flags, and producing reporting outputs suitable for business analysis and compliance review.