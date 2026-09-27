# Present Value Tables — PVIF & PVIFA Reference Guide

Quick reference for discounting cash flows in cost-of-capital / cost-of-debt problems.

## 1. Which table to use

| | When to use it | Formula |
|---|---|---|
| **PVIF** (Present Value Interest Factor) | A **single** cash flow received once, in one specific year — e.g. a bond's redemption value, a deep-discount bond's maturity payout | `PVIF = 1 / (1 + r)^n` |
| **PVIFA** (Present Value Interest Factor of an Annuity) | A cash flow that **repeats** the same amount every year — e.g. equal annual after-tax interest on a debenture | `PVIFA = [1 − (1 + r)^−n] / r` |

PVIFA is nothing new — it's just the PVIF values for years 1 through *n*, added together in advance so you don't have to look up and sum *n* separate factors every time.

## 2. Full factor tables

The complete grids (years 1–30, rates 1%–30%) live alongside this file:

- [`pvif_table.csv`](./pvif_table.csv) — single-amount factors
- [`pvifa_table.csv`](./pvifa_table.csv) — annuity factors
- `PV_Tables_PVIF_PVIFA.xlsx` — the same two tables as live Excel formulas, plus this worked example built in, editable

## 3. Worked example — Cost of Debt by the YTM / IRR method

**Problem:** A company issued 10,000, 10% debentures of ₹100 each on 1.4.2023, maturing on 1.4.2028. The current market price is ₹80. Tax rate is 35%. Find the cost of debt (Kd).

**Step 1 — Identify the cash flows**

| Year | Cash flow | Amount |
|---|---|---|
| 0 | Current market price (P₀) | ₹80 (outflow) |
| 1 to 5 | Interest net of tax: 10% of ₹100 × (1 − 0.35) | ₹6.50 (repeats — use PVIFA) |
| 5 | Redemption value (RV) = Face value | ₹100 (one-time — use PVIF) |

**Step 2 — Discount at two trial rates and find NPV**

| Year | Cash flow (₹) | PVIF/PVIFA @ 10% | PV @ 10% (₹) | PVIF/PVIFA @ 15% | PV @ 15% (₹) |
|---|---|---|---|---|---|
| 0 | (80.00) | 1.000 | (80.00) | 1.000 | (80.00) |
| 1–5 | 6.50 | 3.791 (PVIFA) | 24.64 | 3.352 (PVIFA) | 21.79 |
| 5 | 100.00 | 0.621 (PVIF) | 62.10 | 0.497 (PVIF) | 49.70 |
| **NPV** | | | **+6.74** | | **−8.51** |

**Step 3 — Interpolate for IRR**

```
IRR = L + [ NPV_L / (NPV_L − NPV_H) ] × (H − L)
    = 10% + [ 6.74 / (6.74 − (−8.51)) ] × (15% − 10%)
    = 12.21%
```

**Cost of debt (Kd) ≈ 12.21%** — even though the coupon is 10%, buying the debenture at a discount (₹80 instead of ₹100) pushes the effective yield up.

## 4. Notes on accuracy

- The **wider the gap** between the two trial rates (L and H), the less accurate the straight-line interpolation is — pick trial rates as close to the true IRR as you reasonably can.
- This YTM/present-value method is more accurate than the approximation-method shortcut (`Kd ≈ [I(1−t) + (RV−NP)/n] / [(RV+NP)/2]`), which is a quicker but rougher estimate.
- For a single lump-sum cash flow only (e.g. a zero-coupon / deep-discount bond), you don't need trial and error at all — solve `FV = PV × (1+r)^n` directly for r.
