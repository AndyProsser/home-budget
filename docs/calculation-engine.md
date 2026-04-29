# Calculation Engine

This project depends on a small, deterministic calculation core.

## Design principles

- Keep business logic in pure functions.
- Avoid hard-coded dates and magic constants.
- Ensure every critical formula has unit tests.

## Frequency normalization

All recurring amounts are normalized so that comparisons are reliable.

Constants:

- 26 fortnights per year
- 12 months per year

Formulas:

- Fortnightly amount: fn = amount
- Monthly to fortnightly: fn = amount * 12 / 26
- Annual to fortnightly: fn = amount / 26

Derived values:

- monthlyEquivalent
- annualEquivalent
- fortnightlyAmount

Recommended equivalent conversion from fortnightly:

- monthlyEquivalent = fortnightlyAmount * 26 / 12
- annualEquivalent = fortnightlyAmount * 26

## Ad-hoc handling

Ad-hoc expenses require a user-defined planning basis.

Supported options:

- treat as monthly estimate
- treat as annual estimate
- treat as manual per-cycle entry

## Pay-cycle planning

Given a pay date and expense/income definitions:

1. Resolve current cycle window (payDate to nextPayDate).
2. Calculate total income expected in this cycle.
3. Determine must-pay items due by nextPayDate.
4. Add smoothing (sinking) contributions for non-fortnightly items.
5. Add debt min or target contributions based on strategy.
6. Compute required total and surplus or deficit.
7. Generate recommended transfers by destination account.

Primary outputs:

- totalIncome
- requiredThisCycle
- surplusOrDeficit
- transferPlan (Joint to Bills, Joint to Credit Card, Joint to Savings)
- checklistItems grouped by mustPay, smoothing, debt, savings, discretionary

## Debt projections

For each debt:

1. Apply payment amount for the cycle.
2. Apply interest logic if rate is present.
3. Recompute current balance.
4. Estimate remaining cycles until zero.

Projection outputs:

- updatedBalance
- progressPercent
- projectedDebtFreeDate
- estimatedCyclesRemaining

## Suggested test coverage

- frequency normalization formulas
- cycle boundary handling for due dates
- must-pay selection between two pay dates
- transfer plan aggregation by account
- debt payoff estimate under zero-interest and non-zero-interest cases
