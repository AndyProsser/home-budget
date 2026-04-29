# AI-CONTEXT

This file describes the intent, architecture, and conventions for this repo so AI tools can generate consistent, useful code.

## Project purpose

Build a **browser-based household budgeting and cashflow planner** for a couple with:

- **Fortnightly income**, but **monthly/annual/irregular bills**
- Support for additional income streams with variable payment cycles (week/fn/month/annual/etc...)
- Multiple shared accounts: **Joint**, **Bills**, **Credit Card**, plus optional savings
- Several **0% and structured debts** that need tracking over time

Core goals:

- **Plan per pay cycle** (fortnightly) and see what must be paid now vs later
- **Normalise all expenses** (fn / monthly / annual) into a consistent cashflow model
- **Visualise money flow** (income → accounts → categories → debts/savings/spend)
- **Track debts** (especially 0% deals) and show progress and payoff horizon
- **Support two users** sharing the same household budget

Non-goals (for now):

- No live bank integrations; optional **CSV upload** for transactions only
- No multi-household tenancy; assume **one household per deployment**

---

## Tech stack

**Language & runtime**

- **TypeScript** everywhere (frontend + backend)
- **Node.js** backend, containerised with **Docker**

**Frontend**

- **React** (functional components, hooks)
- **Vite** for bundling/dev server
- **Radix UI** as the base component library
- **Modern, responsive layout**, with:
  - **Dark/light theme support** (system-aware, user-toggle)
  - Accessible components and keyboard navigation

**Backend**

- Node.js + TypeScript
- Simple REST API (or lightweight framework like Express/Fastify)
- Persistence layer can be abstracted (start with in-memory or file-based; later swap to Postgres)

**Testing & quality**

- **Vitest** for unit and integration tests (frontend + backend where possible)
- **ESLint** with TypeScript support
- **Prettier** for formatting
- Aim for **meaningful test coverage** on:
  - Cashflow calculations
  - Pay-cycle planning logic
  - Debt payoff projections

**Tooling**

- **Docker** for local dev and deployment
- **.vscode** settings and recommended extensions to enforce linting, formatting, and test workflows

---

## Core domain concepts

AI should treat these as first-class concepts and model them explicitly.

- **User**: individual login (e.g. Andy, Marnie)
- **Household**: shared budget context for users
- **Account**:
  - Examples: Joint, Bills, Credit Card, Savings
  - Attributes: name, type (bank, credit, savings), isPrimaryIncomeTarget
- **IncomeSource**:
  - Examples: Andy salary, Marnie salary
  - Attributes: amount, frequency (fortnightly/monthly), target account
- **Expense**:
  - Examples: Rent, Power, Health Insurance, Groceries, Netflix, Kids’ school fees
  - Attributes:
    - name
    - category (Rent, Utilities, Debts, Kids, Insurance, Living, Savings, Other)
    - amount
    - frequency (fortnightly, monthly, annual, ad-hoc)
    - due pattern (date or description like “Fri/fn”)
    - paying account
    - flags: isDebt, isZeroInterest, isBumpable (can be deferred)
    - notes
- **DebtDetail** (for expenses that are debts):
  - startingBalance
  - currentBalance
  - minPayment
  - targetPayment
  - rate (optional)
  - startDate
  - expectedEndDate
- **PayCycle**:
  - A single pay period (e.g. every 2 weeks)
  - Attributes: date, status (planned/in_progress/closed), notes
- **PayCycleItem**:
  - A specific expense instance in a pay cycle
  - Attributes: plannedAmount, actualAmount, account, status (planned/paid/bumped/skipped), notes

---

## Key behaviours & calculations

AI should centralise and test these behaviours:

1. **Normalisation of amounts**
   - Convert all expenses to:
     - **fortnightly amount**
     - **monthly equivalent**
     - **annual equivalent**
   - Rules:
     - Fortnightly: `fnAmount = amount`
     - Monthly: `fnAmount = amount * 12 / 26`
     - Annual: `fnAmount = amount / 26`
     - Ad-hoc: use user-defined estimate (treated as monthly or annual)

2. **Pay-cycle planning**
   - For a given pay date:
     - Sum all **fortnightly-normalised** expenses
     - Identify **must-pay** items due before next pay
     - Identify **sinking/smoothing** contributions (e.g. power every 2 months)
     - Compute:
       - Total income this pay
       - Required amount this pay
       - Surplus/deficit
       - Recommended transfers:
         - From Joint → Bills
         - From Joint → Credit Card
         - From Joint → Savings
   - Provide a **checklist** UI:
     - Mark items as Paid / Bumped / Skipped
     - Adjust actual amounts
     - Add unexpected one-off expenses tied to this pay

3. **Lookahead planning**
   - Allow user to view **future pay cycles**:
     - Show upcoming bills and their impact
     - Highlight “heavy” pay cycles (bill spikes)
   - Provide a timeline or table view for multiple upcoming pay cycles

4. **Debt tracking**
   - Track current balance and payments per pay
   - Estimate months to payoff at current payment level
   - Visualise progress with progress bars and “debt-free by” projections
   - Distinguish **0% debts** for motivational wins

5. **Budget flow chart**
   - Provide a **cashflow visual** (Sankey-style or similar):
     - Income → Accounts → Categories → Debts/Savings/Spending
   - Should support toggling between:
     - Fortnightly view
     - Monthly view
     - Annual view

---

## UI views & navigation

AI should structure the frontend around these main views:

1. **Overview**
   - High-level dashboard:
     - Income vs commitments (fn/month/year toggle)
     - Surplus/deficit
     - Category breakdown chart
     - Budget flow chart
     - Debt progress summary

2. **Income**
   - List and edit income sources
   - Set frequency, amount, and target account
   - Show derived fn/month/annual totals

3. **Accounts**
   - List accounts (Joint, Bills, CC, Savings, etc.)
   - Show planned inflows/outflows per pay
   - Optional: **upload transaction data** (CSV) for reconciliation
   - Future-friendly: design API for transaction import, but keep logic simple

4. **Expenses/Bills**
   - Table of all expenses with filters:
     - Category, frequency, account, isDebt, isBumpable
   - Inline editing of amount, frequency, due date, account
   - Flags for “expected/allowed” vs “discretionary”
   - Show normalised fn/month/annual amounts

5. **Debts**
   - List of debts with:
     - Current balance
     - Min and target payments
     - Months remaining (estimate)
     - Progress bars
   - Highlight 0% debts
   - Show total debt and projected payoff horizon

6. **Payments (Pay-cycle planner)**
   - Core working view for each pay:
     - Pay date, income, required amount, surplus/deficit
     - Checklist of items:
       - Must-pay this cycle
       - Sinking funds
       - Debts
       - Savings
       - Discretionary/bumpable items
     - Ability to:
       - Mark as Paid/Bumped/Skipped
       - Add unexpected bills
       - Adjust actual amounts
     - Summary of **recommended transfers** between accounts

---

## Theming & UX

- Use **Radix UI** primitives with a custom theme layer
- Support **dark/light themes**:
  - Respect system preference by default
  - Allow user override
- Keep UX **simple and focused**:
  - Minimal navigation
  - Clear labels and explanations for financial concepts
  - Avoid clutter; prioritise clarity over density

---

## Linting, formatting, and tests

AI should always:

- Use **TypeScript** types and interfaces
- Follow **ESLint** rules (TypeScript + React best practices)
- Use **Prettier** for formatting
- Write **Vitest** tests for:
  - Core calculation utilities (normalisation, pay-cycle planning, debt projections)
  - Critical reducers/hooks
- Aim for **good coverage on business logic**, not just components

Example expectations:

- Prefer pure functions for calculation logic
- Keep React components mostly presentational; move logic into hooks/services
- Avoid any logic that depends on hard-coded dates or magic numbers

---

## .vscode hints

AI should assume the repo includes a `.vscode` folder with:

- `settings.json`:
  - Enable format on save
  - Use workspace versions of TypeScript, ESLint, and Prettier
- `extensions.json`:
  - Recommend:
    - ESLint
    - Prettier
    - TypeScript/JavaScript language features
    - Testing tools (Vitest-related extension if applicable)

---

## Docker & scripts

AI should:

- Provide a **Dockerfile** for running the app (frontend + backend)
- Provide **npm scripts** for:
  - `dev`: run frontend and backend in dev mode
  - `build`: build frontend and backend
  - `test`: run Vitest
  - `lint`: run ESLint
  - `format`: run Prettier

---

## Style & structure preferences

- Prefer **modular, maintainable** code:
  - Separate `domain` (core logic) from `ui` and `infrastructure`
- Use **clear naming** that reflects the domain:
  - `PayCycle`, `Expense`, `DebtDetail`, `Account`, etc.
- Keep files reasonably small and focused
- Favour **composition over inheritance**
