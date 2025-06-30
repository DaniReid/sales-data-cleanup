# Bike Sales Data Cleaning & Preparation Log

**Version:** 1.0 
**Author:** DaniReid
**Last Updated:** 2025-06-30 

## Overview

This document outlines the full cleaning, transformation, and auditing process applied to a raw bike sales dataset. It details each issue encountered, the reasoning behind every decision, and the formulas used to ensure data integrity.

The dataset originally contained:
- Product descriptions (model, color, frame size embedded)
- Customer ages and partially missing age groups
- Incomplete or incorrect financial values
- Redundant date components
- Missing product details

## Key Changes & Fixes

- **Categorized Age Groups** using calculated rules (e.g., `<18`, `18–34`, etc.)
- **Removed Redundant Columns** like separate Day/Month/Year fields
- **Imputed Missing Product Descriptions** via unit cost & pricing pattern analysis
- **Split Product Description** into `Model`, `Color`, and `Frame Size`
- **Created Reference Pricing Sheet** and markup logic
- **Calculated Expected Unit Prices** from markup rates
- **Audited Profit, Revenue, and Cost Columns** using formulas
- **Flagged and Corrected Mismatches** between actual and expected values
- **Created Documentation and Profit Audit Sheet** for transparency and traceability

## Sheets in Workbook

- `Cleaned Data`: Final validated dataset
- `Pricing Reference & Audit`: Lookup values and markup rules
- `Profit Audit`: Expected vs actual comparison

## Change Log

| Version | Date       | Description                             |
|---------|------------|-----------------------------------------|
| 1.0     | 2025-06-30 | Initial data cleaning and audit log     |

---

## Usage Notes

This dataset is now ready for use in reporting, dashboarding, or further analysis. All changes are documented and reproducible.
