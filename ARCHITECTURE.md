# Home Budgeting APP

## 1. High-level concept

A small web app with:

- **Multi‑user, shared household profile**
- **Fortnightly engine** that normalises everything (monthly, annual, irregular) into per‑pay amounts
- **Pay‑cycle checklist**: “Here’s what this pay needs to cover”
- **Account routing**: “From Joint → Bills / CC / Savings”
- **Debt tracker** for 0% and other structured repayments
- **Motivational visuals**: flow charts, progress bars, category breakdowns

Think of it as: *“Every dollar has a job, every pay has a plan.”*

---

## 2. Core data model

### **Entities**

- **User**
  - **Fields:** id, name, email, role (Owner/Partner)
- **Household**
  - **Fields:** id, name, timezone, default pay frequency (fn)
- **Account**
  - **Fields:** id, name (Joint, Bills, CC, etc.), type (bank, credit, savings), is_primary_income_target (bool)
- **IncomeSource**
  - **Fields:** id, name (Andy, Marnie), amount, frequency (fn/month), account_id
- **Expense**
  - **Fields:**  
    - name (Rent, Health Ins, Groceries, etc.)  
    - category (Rent, Utilities, Debts, Kids, Insurance, Living, Savings, Other)  
    - amount  
    - frequency (fn, monthly, annual, ad‑hoc)  
    - due_day / due_pattern (e.g. 3rd, 28th, “Fri/fn”)  
    - account_id (where it’s paid from)  
    - is_debt (bool)  
    - is_zero_interest (bool)  
    - notes
- **DebtDetail** (for is_debt = true)
  - **Fields:** starting_balance, current_balance, min_payment, target_payment, rate (optional), start_date, expected_end_date
- **PayCycle**
  - **Fields:** date, status (planned / in_progress / closed), notes
- **PayCycleItem**
  - **Fields:** pay_cycle_id, expense_id, planned_amount, actual_amount, account_id, status (planned / paid / bumped / skipped), notes
- **Note**
  - **Fields:** scope (household / expense / pay_cycle), text

---

## 3. Calculation engine (the important bit)

### **Normalisation to per‑pay**

For each expense:

- **Fortnightly:**  
  \(\text{fn\_amount} = \text{amount}\)
- **Monthly:**  
  \(\text{fn\_amount} = \text{amount} \times 12 / 26\)
- **Annual:**  
  \(\text{fn\_amount} = \text{amount} / 26\)
- **Ad‑hoc:**  
  user‑defined estimate (e.g. “Medical $250/m → treat as monthly”)

Store:

- **fn_amount**
- **monthly_equivalent**
- **annual_equivalent**

This powers:

- fn | month | annual views  
- “How much this pay needs to cover”  
- Category charts

---

## 4. Screens / flows

### 4.1 Dashboard

For a selected **time view** (fn / month / year):

- **Top strip:**
  - **Income vs Commitments:** bar or gauge
  - **Surplus / Shortfall:** big number
- **Charts:**
  - **Budget Flow (Sankey‑style):**  
    Income → Accounts → Categories → Debts/Savings/Spending
  - **Category Breakdown:** donut (Rent, Utilities, Debts, Kids, etc.)
  - **Debt Progress:** stacked bar or list with progress bars
- **Quick facts:**
  - Total 0% debt remaining  
  - Months until debt‑free (at current pace)  
  - Savings per year at current settings  

### 4.2 Pay-cycle checklist (the “fn sheet”, but sane)

For each pay date:

- **Header:**
  - Pay date  
  - Total income  
  - Required this pay (sum of fn_amounts)  
  - Surplus / deficit
- **Sections:**
  - **Must‑pay this cycle** (due by next pay):  
    - Rent, loans, direct debits, etc.  
    - Checkbox + actual amount + account
  - **Sinking / smoothing funds** (fn share of monthly/annual bills)
  - **Debts:**  
    - Min payment  
    - Target payment  
    - Option to add “extra this pay”
  - **Savings & goals**
- **Actions:**
  - Mark item as **Paid / Bumped / Skipped**
  - Adjust actual amount
  - Add **unexpected bill** (one‑off expense tied to this pay)
- **Account routing summary:**
  - From **Joint**:  
    - → Bills: $X  
    - → CC: $Y  
    - → Savings: $Z  
  - Remaining in Joint: $R

This is the screen Marnie would print today—but now it’s live, tickable, and always in sync.

### 4.3 Expenses & bills

- **Table view** with filters:
  - Category, account, frequency, is_debt
- **Inline editing**:
  - Amount, frequency, due date, account
- **Flags:**
  - “Expected/allowed” vs “discretionary”
- **Bump logic:**
  - Mark an expense as “bumpable” (e.g. Entertainment, Clothes)  
  - In pay‑cycle view, you can bump it to next pay with one click

### 4.4 Debts

- **List of debts** with:
  - Name, current balance, min payment, target payment, fn cost, months remaining
- **Visuals:**
  - Progress bar per debt  
  - Combined “debt‑free by” projection
- **Controls:**
  - Set **snowball / avalanche** strategy later if you want  
  - Mark 0% debts separately for psychological wins

### 4.5 Accounts

- Define:

  - Joint  
  - Bills  
  - CC  
  - Any others (Savings, Emergency, etc.)

- For each account:
  - Planned inflows per pay  
  - Planned outflows per pay  
  - Net per pay  
  - (Optional later: sync balances via bank export/manual entry)

---

## 5. Tech stack (aligned with you)

Given you’re on macOS + Linux and like modern tooling:

- **Frontend:**
  - **React + TypeScript**
  - **Vite** for dev/build
  - **Component lib:** something clean like MUI or Mantine (for charts, tables, modals)
  - **Charts:** Recharts or ECharts (Sankey, donut, progress)
- **Backend:**
  - **Node.js + TypeScript**
  - **Fastify** or **Express**
  - **PostgreSQL** (great for relational budgeting data)
- **Auth & multi‑user:**
  - Simple email/password to start; later OAuth if you want
- **Deployment:**
  - Dockerised, so you can run it on your homelab or a small VPS

---

## 6. How we migrate from your current sheet

1. **Export your current “Budget” + “Calendar” + “Budget (BUY)” into CSVs**
2. Map them into:
   - Income sources  
   - Expenses (with category, frequency, account, due date)  
   - Debts (with balances and arrangements)
3. Seed the database with that data.
4. Use the normalisation engine to compute fn/month/annual.

We can even keep a **“Raw import”** view so you can sanity‑check that nothing got lost in translation.

---

## 7. What I can do next for you

If you like this direction, next step I can:

- Define the **exact database schema** (CREATE TABLEs)
- Sketch the **API endpoints** (REST routes)
- Outline the **React page/component structure**
- Then, if you want, generate:
  - Initial **schema + seed script** based on your current budget
  - A **basic working UI** for:
    - Expenses list  
    - Pay‑cycle checklist  
    - Dashboard with at least one flow chart + one breakdown chart
