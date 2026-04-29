# Roadmap

This roadmap is designed to move from planning to a usable first release with stable budgeting logic.

## Stage 0: Project foundation

Objective:

- Establish repo structure, tooling, and quality baseline.

Deliverables:

- App skeleton for frontend and backend in TypeScript.
- Shared domain package or folder for core models and calculations.
- ESLint, Prettier, Vitest, and CI checks.
- Dockerfile and basic run scripts.

Exit criteria:

- Local dev startup works.
- Lint, test, and format commands run successfully.
- CI validates pull requests.

## Stage 1: Domain model and calculation engine

Objective:

- Implement trusted core logic before building complex UI flows.

Deliverables:

- Typed domain models for Household, Account, IncomeSource, Expense, DebtDetail, PayCycle, and PayCycleItem.
- Frequency normalization utilities (fortnightly, monthly, annual, ad-hoc planning basis).
- Pay-cycle planning function with checklist grouping and transfer recommendations.
- Debt projection utilities with zero-interest and interest-bearing paths.
- Unit tests for all critical formulas and edge cases.

Exit criteria:

- Core calculation tests pass with strong coverage.
- Output shapes are stable and documented.

## Stage 2: Backend API baseline

Objective:

- Expose domain workflows through a clean API contract.

Deliverables:

- REST routes for accounts, incomes, expenses, debts, pay cycles.
- Validation layer for request payloads.
- In-memory or file-based persistence adapters.
- API route for pay-cycle plan preview.

Exit criteria:

- CRUD and planning endpoints functional.
- Error handling and validation responses are consistent.

## Stage 3: Frontend shell and navigation

Objective:

- Create the app frame and shared UI primitives.

Deliverables:

- React app shell with responsive layout.
- Route structure for Overview, Income, Accounts, Expenses, Debts, and Payments.
- Theme support (light, dark, system).
- Base component layer using Radix primitives.

Exit criteria:

- Navigation and layout are stable on desktop and mobile.
- Theme switching works and persists.

## Stage 4: Core data management screens

Objective:

- Allow users to fully configure household budgeting inputs.

Deliverables:

- Income management screen.
- Accounts management screen.
- Expenses table with filters and inline editing.
- Debt details editing for debt-linked expenses.

Exit criteria:

- Users can enter and edit all required setup data.
- Normalized values are visible and accurate.

## Stage 5: Pay-cycle planner

Objective:

- Deliver the highest-value workflow: planning each pay cycle.

Deliverables:

- Payments view with current cycle summary.
- Checklist groups: must-pay, smoothing, debts, savings, discretionary.
- Item status updates (planned, paid, bumped, skipped).
- Actual amount capture and one-off unexpected bill entry.
- Recommended transfer summary between accounts.

Exit criteria:

- A full pay cycle can be planned and tracked end to end.
- Surplus or deficit output is trustworthy and explainable.

## Stage 6: Insights and lookahead

Objective:

- Improve decision support through forward visibility.

Deliverables:

- Future pay-cycle timeline or table.
- Heavy-cycle highlighting.
- Overview charts for category breakdown and debt progress.
- Budget flow visualization (Sankey-style or equivalent).

Exit criteria:

- Users can identify upcoming pressure points before they happen.

## Stage 7: Reconciliation and import

Objective:

- Improve confidence by comparing plan versus real transactions.

Deliverables:

- CSV upload flow for transactions.
- Basic mapping and validation for imported rows.
- Reconciliation helpers against pay-cycle actuals.

Exit criteria:

- Users can import transactions and reconcile key differences.

## Stage 8: Hardening and first release

Objective:

- Prepare for reliable everyday use.

Deliverables:

- Integration tests for key workflows.
- Accessibility pass for keyboard and screen reader basics.
- Performance and error monitoring baseline.
- Release checklist and versioned changelog.

Exit criteria:

- Stable v1 release candidate tagged.
- Documentation and onboarding are complete.

## Suggested implementation order by value

1. Stage 0
2. Stage 1
3. Stage 2
4. Stage 3
5. Stage 5
6. Stage 4
7. Stage 6
8. Stage 7
9. Stage 8

## Milestone view

- Milestone A: Stages 0 to 2 (engineering baseline)
- Milestone B: Stages 3 to 5 (usable core product)
- Milestone C: Stages 6 to 8 (insights, confidence, release quality)

## Milestone tracker

Use this as a lightweight delivery board. Update target week and status during planning reviews.

| Milestone   | Stages | Target week | Status      |
| ----------- | ------ | ----------- | ----------- |
| Milestone A | 0 to 2 | Week 2      | Not started |
| Milestone B | 3 to 5 | Week 6      | Not started |
| Milestone C | 6 to 8 | Week 9      | Not started |

Status options:

- Not started
- In progress
- At risk
- Blocked
- Done
