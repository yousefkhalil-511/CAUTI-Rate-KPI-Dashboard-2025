# CAUTI-Rate-KPI-Dashboard-2025
This project tracks the Catheter-Associated Urinary Tract Infection (CAUTI) rate across hospital departments throughout 2025. It turns raw monthly infection counts into a KPI dashboard so infection-prevention staff can monitor trends, spot departments or months that fall outside normal variation, and support ongoing quality-improvement efforts.
## File

- `CAUTI_Data_2025.xlsx`

## How It Was Built

- **Python** was used to generate the underlying dataset (synthetic/sample monthly infection counts and patient-days by department).
- **Excel native formulas** (no macros, no external add-ins) were used to build the summary tables, statistical process control (SPC) calculations, and charts, so the workbook stays fully editable and recalculates automatically when source data changes.

## Workbook Structure

The workbook contains three sheets:

### 1. `Dataset`
The raw, transaction-level data — one row per Month × Department.

| Column | Description |
|---|---|
| Month | Calendar month (January–December) |
| Quarter | Fiscal quarter (Q1–Q4) |
| Department | Medical ICU, Surgical ICU, Medical Ward, Surgical Ward, NICU, PICU |
| Infection Observed | Count of CAUTI cases observed that month |
| Denominator | Catheter-days (or patient-days) for that month/department |
| Infection Rate | `=(Infection Observed / Denominator) * 1` — the raw monthly infection rate per department |

### 2. `Analysis`
Formula-driven summary tables built on top of `Dataset`, using `VLOOKUP`, `SUMIF`, `AVERAGE`, and `STDEV.S`:

- **SPC Chart Table** — hospital-wide monthly infection rate compared against:
  - **Mean** — the average infection rate across all 12 months
  - **UCL (Upper Control Limit)** — Mean + 3×standard deviation
  - **LCL (Lower Control Limit)** — Mean − 3×standard deviation
  
  This is a standard statistical process control (SPC) setup used to flag months where the infection rate moved outside expected random variation.

- **Per-Department Trend Tables** — six side-by-side tables (Medical ICU, Medical Ward, Surgical ICU, Surgical Ward, NICU, PICU), each showing the monthly infection rate and the month-over-month **Variance**.

### 3. `Dashboard`
The visual KPI layer, built entirely from native Excel charts linked to the `Analysis` sheet:

- **1 SPC line chart** — hospital-wide monthly infection rate with Mean/UCL/LCL bands, for at-a-glance monitoring of whether the process is in control.
- **6 department bar charts** — one per department, comparing monthly infection rate against variance, to identify which units are driving overall trends.

All charts update automatically when the `Dataset` sheet is edited, since every downstream cell is formula-driven.

## How to Use

1. Update or add rows in the `Dataset` sheet with new monthly data (Month, Quarter, Department, Infection Observed, Denominator).
2. The `Infection Rate` column, the `Analysis` sheet tables, and all `Dashboard` charts recalculate automatically — no manual steps required.
3. Review the `Dashboard` sheet for:
   - Any month where the hospital-wide rate crosses the UCL/LCL (possible special-cause variation worth investigating).
   - Departments with rising trends or high variance month-to-month.

## Notes / Assumptions

- Data for 2025 in this workbook is sample/generated data (created via Python), intended to demonstrate the KPI dashboard methodology rather than represent actual patient records.
- SPC control limits (UCL/LCL) are calculated using ±3 standard deviations from the annual mean, a common convention for control charts; adjust the multiplier in the `Analysis` sheet formulas if a different confidence threshold is desired.
