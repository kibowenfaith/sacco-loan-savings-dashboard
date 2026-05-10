# Data Dictionary — Jua Kali SACCO Analytics Project

All datasets are synthetic and generated for portfolio demonstration purposes.

---

## members_raw.csv

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| member_id | String | Unique member identifier | MEM0001 |
| full_name | String | Member full name (Kenyan names) | James Odhiambo |
| gender | String | Male / Female | Male |
| branch | String | SACCO branch | Nairobi |
| occupation | String | Member's occupation | Teacher |
| phone_number | String | Kenyan mobile number | 0722123456 |
| join_date | Date | Date member joined the SACCO | 2020-03-15 |
| monthly_contribution_kes | Integer | Monthly savings contribution in KES | 2000 |
| member_status | String | Active / Inactive / Suspended | Active |
| national_id | String | Kenya National ID number | 25431876 |

---

## loans_raw.csv

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| loan_id | String | Unique loan identifier | LN00001 |
| member_id | String | FK → members_raw | MEM0001 |
| branch | String | Branch where loan was issued | Nairobi |
| loan_product | String | Type of loan | Asset Finance |
| principal_amount_kes | Integer | Original loan amount in KES | 500,000 |
| interest_rate_annual | Float | Annual interest rate (decimal) | 0.15 |
| loan_term_months | Integer | Loan duration in months | 36 |
| disbursement_date | Date | Date loan was paid out | 2023-04-10 |
| maturity_date | Date | Expected repayment completion date | 2026-04-10 |
| monthly_installment_kes | Float | Monthly repayment amount in KES | 17,332.50 |
| outstanding_balance_kes | Float | Remaining balance as at Dec 2024 | 245,000 |
| loan_status | String | Performing / PAR 30 / PAR 60 / PAR 90 / Written Off / Cleared | Performing |
| loan_officer | String | Assigned loan officer code | LO05 |
| collateral_type | String | Security held for the loan | Log Book |

**Loan Products & Interest Rates:**

| Product | Rate | Max Term | Notes |
|---------|------|----------|-------|
| Development Loan | 14% | 36 months | General purpose |
| Emergency Loan | 18% | 12 months | Fast-turnaround |
| School Fees Loan | 12% | 12 months | Education finance |
| Logbook Loan | 16% | 48 months | Vehicle-secured |
| Asset Finance | 15% | 60 months | Equipment/machinery |
| Salary Advance | 10% | 6 months | Payroll-backed |
| Top-Up Loan | 15% | 24 months | Existing borrower top-up |

**PAR Definitions (SASRA standard):**
- **Performing** — repayments up to date or <30 days overdue
- **PAR 30** — 30–60 days past due
- **PAR 60** — 60–90 days past due
- **PAR 90** — 90+ days past due (high risk)
- **Written Off** — deemed unrecoverable
- **Cleared** — fully repaid

---

## savings_transactions.csv

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| transaction_id | String | Unique transaction ID | SAV000001 |
| member_id | String | FK → members_raw | MEM0042 |
| branch | String | Branch where transaction occurred | Kisumu |
| transaction_date | Date | Date of transaction | 2023-06-03 |
| transaction_type | String | Deposit / Withdrawal | Deposit |
| amount_kes | Float | Amount (negative for withdrawals) | 2000 |
| balance_after_kes | Float | Running member balance after transaction | 18,400 |
| notes | String | Transaction description | Monthly contribution |

---

## repayment_schedule.csv

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| payment_id | String | Unique payment record ID | PAY0000001 |
| loan_id | String | FK → loans_raw | LN00023 |
| member_id | String | FK → members_raw | MEM0042 |
| branch | String | Branch | Mombasa |
| due_date | Date | When installment was due | 2023-07-10 |
| payment_date | Date | When payment was actually made (null if unpaid) | 2023-07-12 |
| installment_due_kes | Float | Expected payment amount | 17,332.50 |
| amount_paid_kes | Float | Actual amount received | 17,332.50 |
| days_late | Integer | Number of days payment was overdue | 2 |
| payment_status | String | Paid on Time / Late Payment / Partial Payment / Defaulted | Paid on Time |

---

## Key Metrics Definitions

| Metric | Formula |
|--------|---------|
| PAR Ratio | Outstanding balance of PAR loans ÷ Total outstanding balance |
| Repayment Rate | Total amount paid ÷ Total installments due |
| Savings-to-Loan Ratio | Total savings mobilised ÷ Total outstanding loan portfolio |
| Loan Utilisation Rate | Member's outstanding loan ÷ Member's total savings balance |
| Contribution Tier | Bronze ≤1,000 / Silver 1,001–2,000 / Gold 2,001–3,000 / Platinum >3,000 (KES/month) |
