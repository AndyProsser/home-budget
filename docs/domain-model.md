# Domain Model

This document defines the core entities and relationships used throughout the app.

## User

Represents one person who can access the household budget.

Suggested fields:

- id
- name
- email
- role (Owner, Partner)

## Household

Shared budgeting context for users.

Suggested fields:

- id
- name
- timezone
- defaultPayFrequency (fortnightly by default)

## Account

Represents where money is held or paid from.

Examples:

- Joint
- Bills
- Credit Card
- Savings

Suggested fields:

- id
- name
- type (bank, credit, savings)
- isPrimaryIncomeTarget

## IncomeSource

Defines recurring income entering the system.

Examples:

- Salary A
- Salary B
- Side income

Suggested fields:

- id
- name
- amount
- frequency (weekly, fortnightly, monthly, annual)
- targetAccountId

## Expense

Represents a planned outgoing amount.

Suggested fields:

- id
- name
- category (Rent, Utilities, Debts, Kids, Insurance, Living, Savings, Other)
- amount
- frequency (fortnightly, monthly, annual, ad-hoc)
- duePattern (specific day or descriptive pattern)
- accountId
- isDebt
- isZeroInterest
- isBumpable
- notes

Derived fields (computed):

- fortnightlyAmount
- monthlyEquivalent
- annualEquivalent

## DebtDetail

Attached to an expense where isDebt is true.

Suggested fields:

- expenseId
- startingBalance
- currentBalance
- minPayment
- targetPayment
- rate (optional)
- startDate
- expectedEndDate (optional)

## PayCycle

Represents one planning period (usually every 14 days).

Suggested fields:

- id
- householdId
- payDate
- status (planned, in_progress, closed)
- notes

## PayCycleItem

Represents one expense instance in a pay cycle.

Suggested fields:

- id
- payCycleId
- expenseId
- plannedAmount
- actualAmount
- accountId
- status (planned, paid, bumped, skipped)
- notes

## Note

Simple freeform notes tied to household, expense, or pay cycle scope.

Suggested fields:

- id
- scopeType
- scopeId
- text
- createdByUserId
- createdAt

## Relationship summary

- Household has many Users.
- Household has many Accounts.
- Household has many IncomeSources.
- Household has many Expenses.
- Expense optionally has one DebtDetail.
- Household has many PayCycles.
- PayCycle has many PayCycleItems.
- PayCycleItem points to one Expense.
