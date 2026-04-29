# Views and Flows

## Core user journeys

## 1. Initial setup journey

1. Add accounts.
2. Add income sources.
3. Add expenses.
4. Mark debt expenses and enter debt details.
5. Generate first pay-cycle plan.

Success signal:

- User can produce a complete first-cycle checklist with transfer suggestions.

## 2. Routine pay-cycle journey

1. Open current pay cycle.
2. Review required amount and surplus or deficit.
3. Process checklist groups in priority order.
4. Mark items as paid, bumped, or skipped.
5. Enter actual amounts and one-off expenses.
6. Confirm transfer summary and close cycle.

Success signal:

- End-of-cycle status is complete and variances are visible.

## 3. Debt optimization journey

1. Open Debts view.
2. Review balances and months remaining.
3. Compare min versus target payments.
4. Adjust target payment.
5. Validate payoff horizon change.

Success signal:

- User can make informed debt payment trade-offs quickly.

## Screen-level expectations

Overview screen:

- Header KPIs: income, commitments, surplus or deficit.
- Visual area: category breakdown, debt progress, budget flow.
- Heavy-cycle indicator for near-term risk.

Expenses screen:

- Filterable table with inline edits.
- Fast actions: mark bumpable, set due pattern, assign account.
- Computed columns for normalized values.

Payments screen:

- Summary card for cycle totals.
- Checklist sections ordered by urgency.
- Transfer recommendation block pinned near completion actions.

## State handling

For each major view, define and style:

- loading
- empty
- partial data
- validation error
- save failure
- success confirmation
