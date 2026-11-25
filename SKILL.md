---
name: csv-excel-auditor
description: Audit and clean messy CSV or Excel spreadsheets. Use when the user uploads a spreadsheet or asks to check for missing values, duplicates, broken formulas, date/currency inconsistencies, outliers, or prepare raw tabular data for analysis.
---

# CSV & Excel Data Auditor

Perform a fast, structured data-quality audit on any uploaded CSV, TSV, or Excel workbook before analysis or executive reporting.

## Workflow
1. **Inspect structure**: Identify sheet names, row/column counts, inferred data types, and header row offset (flag merged cells or multi-row headers).
2. **Run 6 mandatory quality checks**:
   - **Missing values**: Count and percentage of nulls/blanks per column; flag columns >5% missing.
   - **Duplicate records**: Detect exact duplicate rows and near-duplicate primary keys (e.g., same ID or trimmed lowercase name/date).
   - **Type & format drift**: Catch mixed date formats (`MM/DD/YYYY` vs `DD/MM/YYYY`), text-formatted numbers (`"$1,200"` or trailing spaces), and mixed currencies.
   - **Formula & error cells** (Excel): Flag `#REF!`, `#DIV/0!`, `#VALUE!`, `#NAME?`, `#N/A`, and hardcoded overrides inside formula columns.
   - **Outliers & impossible values**: Flag negative quantities/prices where invalid, future dates where invalid, or statistical outliers (`> 3x IQR` or `|z| > 3`).
   - **Referential sanity**: Verify subtotal/total rows are not mixed into raw data rows (preventing double-counting).
3. **Score readiness**: Assign a `Data Readiness Score (0–100)` using deductions: `-15` for formula errors, `-10` for duplicate keys, `-10` for mixed currencies/dates, `-5` per column with >10% nulls.
4. **Deliver output in two parts**:
   - **Audit Summary Table**: Issue category, affected column/sheet, exact row count, severity (`Critical`, `Warning`, `Info`), and recommended fix.
   - **Clean Transformation Script / Cleaned File**: If code execution is available, output a cleaned `.csv`/`.xlsx` preserving the original file untouched; otherwise provide exact Excel formulas or Python/pandas cleaning commands.

## Guardrails
- Never silently drop rows or impute missing financial numbers without explicitly listing every affected row and asking for confirmation.
- Never invent or estimate missing values from general knowledge.
