
# Bike Sales Data Cleaning & Preparation Log

**Version:** 1.0 
**Author:** DaniReid
**Last Updated:** 2025-06-30

## Purpose
To document all data cleaning, transformation, and validation steps performed on the raw bike sales dataset to ensure its integrity and analytical usability.

## Dataset Overview
The original dataset contained raw sales records for various bike models sold by a retailer. Key columns included:
- `Product Description` (e.g., “Mountain-200 Black, 46”)
- `Customer Age` and separate Day/Month/Year columns
- `Customer Age Group` (partially filled)
- `Order Quantity`, `Unit Cost`, `Unit Price`, `Cost`, `Revenue`, `Profit`

The data exhibited multiple issues: missing values, inconsistent categorical logic, redundant fields, and numeric mismatches.

---

## Detailed Step-by-Step Actions & Reasoning

### 1. Initial Column Formatting
- **Action:** Adjusted column widths and spacing so each header and cell value was fully visible.
- **Reasoning:** Ensures no hidden or truncated values, making review and manual inspections reliable.

### 2. Enabling Change Documentation
- **Action:** Decided on a lightweight log format (markdown bullets) to record: what changed, why it changed.
- **Reasoning:** Provides reproducibility and transparency without overwhelming detail.

### 3. Identifying and Resolving Age Group Gaps
- **Action:** Noticed some rows lacked `Customer Age Group` despite valid `Customer Age` values.
- **Action:** Inserted a helper column with nested `IF(AND())` logic to derive age groups:
  - Youth: <18
  - Young Adults: 18–34
  - Adults: 35–64
  - Older Adults: ≥65
- **Action:** Detected a logic gap where ages 25–28 fell outside the original “Youth <25” and “Young Adults 18–34” definitions.
- **Action:** Standardized “Youth” to `<18` and “Young Adults” to `18–34` to close the gap.
- **Reasoning:** Ensures every customer age maps to exactly one group; eliminates ambiguous categorizations.

### 4. Removing Redundant Date Parts
- **Action:** Reviewed separate Day/Month/Year columns next to a full `Date` field.
- **Action:** Validated `Date` completeness by sorting and checking for blanks; confirmed parseable values.
- **Action:** Dropped the Day/Month/Year columns to reduce redundancy.
- **Reasoning:** A single `Date` column is easier to maintain; individual components can be derived when needed.

### 5. Recovering Missing Product Descriptions
- **Action:** Found a row (J21) with no `Product Description` but filled financial metrics.
- **Action:** Matched on `Unit Cost`, `Unit Price`, `Revenue`, and `Profit` ratios against known rows to impute the missing description (“Mountain-200 Black, 46”).
- **Reasoning:** Prevents loss of valuable sales record; demonstrates data-forensics capability.

### 6. Decomposing `Product Description`
- **Action:** Created three new columns: `Model`, `Color`, `Frame Size` via formulas:
  - Frame Size: `=TRIM(RIGHT(J2, LEN(J2)-FIND(",",J2)))`
  - Color: extracted last word before comma
  - Model: extracted leading substring before color
- **Reasoning:** Normalized product attributes support flexible grouping and filtering in analysis.

### 7. Building a Pricing Reference Sheet
- **Action:** On a new sheet named **Pricing Reference & Audit**, compiled unique combinations of `Model`+`Color` with observed `Unit Cost` and calculated markup multipliers.
- **Action:** Calculated markup percentages: ~77.8% for Mountain‑100 series; ~83.3% for Mountain‑200 to 500 series.
- **Reasoning:** Creates a single source of truth for correct pricing logic; simplifies bulk calculations.

### 8. Automating Expected Price Calculations
- **Action:** In the main sheet, added formulas using `XLOOKUP` to fetch markup and compute **Expected Unit Price**: `=Unit Cost * Markup`.
- **Action:** Added **Expected Cost**, **Expected Revenue**, **Expected Profit** columns:
  - `Expected Cost = Unit Cost × Quantity`
  - `Expected Revenue = Unit Price × Quantity`
  - `Expected Profit = Expected Revenue − Expected Cost`
- **Reasoning:** Derives all financial metrics from authoritative inputs; prevents manual entry errors.

### 9. Mismatch Identification & Correction
- **Action:** Introduced `Mismatch` flags with:
  ```excel
  =IF(ABS(Expected - Actual) > 1, "Mismatch", "")
  ```
- **Action:** Filtered for mismatches, then:
  - Corrected zero unit-price entries (e.g., F8) by applying reference prices.
  - Fixed rows where `Unit Cost` wrongly equaled `Unit Price` (e.g., R5), restoring cost to $1,252 for Mountain‑200 Black.
  - Recalculated `Cost`, `Revenue`, and `Profit` using formula logic.
  - Corrected profit discrepancies (e.g., Mountain‑400W profit mismatch of $3.44).
- **Reasoning:** Systematic QA ensures all records adhere to defined pricing and profit rules; documents each fix for auditability.

### 10. Final Audit & Export Preparation
- **Action:** Created a **Profit Audit** sheet summarizing actual vs expected metrics and mismatch reasons for review.
- **Action:** Logged each correction with explanations in the documentation sidebar.
- **Reasoning:** Provides clear audit trail and ensures completeness before using the data for reporting or analysis.

---

## Conclusion
- Fully cleaned, de-duplicated, and validated bike sales dataset.
- Comprehensive documentation of every transformation, formula, and decision.
- Dataset and logs ready for advanced analysis or business reporting.

---

## Change History

| Version | Date       | Description                             |
|---------|------------|-----------------------------------------|
| 1.0     | 2025-06-30 | Initial documentation and cleanup log.  |
