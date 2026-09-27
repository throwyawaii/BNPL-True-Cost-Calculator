# BNPL True Cost Calculator

A single-page, self-contained HTML/JS tool that shows the real cost of buying a
device on Buy-Now-Pay-Later (BNPL), compared to paying the market price
upfront.

## What it does

Given:

- **Market price** — the device's normal cash price
- **Deposit** — the upfront BNPL deposit
- **Installment amount** — either a **daily** or **weekly** deposit amount
- **Timeframe** — total number of days the plan runs for (defaults to 365)

It calculates:

| Metric | Meaning |
|---|---|
| **Total paid** | Deposit + (installment amount × number of installments) |
| **# of installments** | Days (if daily) or weeks (if weekly) in the timeframe |
| **Total installments paid** | Sum of all recurring payments, excluding the deposit |
| **Extra paid vs market price** | Total paid − market price |
| **Deviation from market price** | Extra paid as a % of market price |
| **Effective interest on financed amount** | Extra paid ÷ (market price − deposit), as a % |
| **Annualised interest rate (APR-style)** | The above, scaled to a 365-day year if the timeframe isn't a year |

"Financed amount" is treated as `market price − deposit` — i.e. the portion of
the device's cost you're effectively borrowing. If you'd rather have interest
calculated against the full market price instead, that's a one-line change
(see below).

## Usage

1. Open `bnpl-calculator.html` in any modern browser (or the published
   artifact link).
2. Enter market price, deposit, and installment amount.
3. Toggle **Daily / Weekly** to match your BNPL plan.
4. Adjust the timeframe if it isn't 365 days.
5. Results update live as you type.

No build step, no dependencies, no internet connection required — it's a
single HTML file with inline CSS and JS.

## Customizing

All logic lives in the `calculate()` function inside the `<script>` tag:

- To base interest on full market price instead of the financed gap, change:
  ```js
  const financedAmount = Math.max(marketPrice - deposit, 0);
  ```
  to:
  ```js
  const financedAmount = marketPrice;
  ```
- To support monthly installments, add a third toggle button and a case in
  the `numInstallments` calculation (`timeframeDays / 30`, roughly).

## Notes

- Figures are illustrative. Real BNPL plans may include late fees, insurance
  add-ons, or other charges not captured by these four inputs.
- Currency is displayed as KES but the math is currency-agnostic — just enter
  numbers in whatever currency your plan uses.
