# PNB MIS — Management Information System
### Punjab National Bank · Banking Operations Platform

---

## Overview

PNB MIS is a fully functional, single-file Management Information System prototype built for Punjab National Bank. It demonstrates a complete software stack — a dynamic frontend, an in-browser database engine with real persistence, and a role-based access control system — all delivered as a single HTML file requiring no installation, no server, and no internet connection.

The system covers all five MIS functions defined in the assignment: Customer Management, Transaction Processing, Risk Management, Loan & Credit Management, and Financial Management, across three MIS levels — Strategic (ESS), Tactical, and Operational (TPS).

---

## Quick Start

1. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
2. The loading screen will initialise the database automatically
3. Use any demo account from the login screen to sign in

No npm, no Python, no backend server required.

---

## Demo Accounts

| Employee ID | Password | Role    | Scope                          |
|-------------|----------|---------|--------------------------------|
| EMP001      | cmd123   | CMD     | Full access · All India        |
| EMP002      | ed123    | ED      | Executive Director             |
| EMP003      | gm123    | GM      | General Manager · North Zone   |
| EMP004      | dgm123   | DGM     | Deputy General Manager · North |
| EMP005      | rm123    | RM      | Regional Manager · East Zone   |
| EMP006      | bm123    | BM      | Branch Manager · Delhi         |
| EMP007      | bm456    | BM      | Branch Manager · Mumbai        |
| EMP008      | bo123    | BO      | Banking Officer · Delhi        |
| EMP009      | tel123   | TELLER  | Teller · Delhi Branch          |
| EMP010      | aud123   | AUDITOR | Internal Auditor · Head Office |

> **Tip:** Click any role chip on the login screen to auto-fill credentials.

---

## Technical Architecture

### Frontend
- Pure HTML5, CSS3, and vanilla JavaScript — no frameworks, no build tools
- Single-page application with client-side routing
- Responsive layout with collapsible sidebar navigation
- Inline cell editing on all data tables (click any ✏️ cell)
- Modal dialogs for all create/edit/view operations
- Real-time toast notifications for all actions

### Database Layer
- Custom pure-JavaScript in-memory database engine (no SQL.js, no WebAssembly)
- Full CRUD operations: `insert`, `update`, `remove`, `find`, `findOne`
- Auto-incrementing primary keys with sequence tracking
- `localStorage` persistence — all changes survive page refresh
- Cross-tab real-time sync via the browser `storage` event
- Export database as a timestamped `.json` file
- Import any previously exported `.json` file to restore state

### Authentication & Session
- Employee ID + password login with hash-based credential verification (djb2 hash)
- In-memory session object storing role, branch scope, and zone
- Automatic last-login timestamp update on sign-in
- Full audit trail: every login, logout, and data operation is recorded

### Role-Based Access Control (RBAC)
Two layers of access control are enforced on every navigation item and every action button:

**Module-level permissions** (`PERMS`) — which sections of the application a role can see:

| Role    | Accessible Modules |
|---------|--------------------|
| CMD     | All 13 modules |
| ED      | All except User Management |
| GM      | Dashboard, Customers, Transactions, Loans, Risk, Fraud, Financial, Branches, Complaints, Reports |
| DGM     | Dashboard, Customers, Transactions, Loans, Risk, Fraud, Financial, Branches, Complaints |
| RM      | Dashboard, Customers, Transactions, Loans, Risk, Branches, Complaints |
| BM      | Dashboard, Customers, Transactions, Loans, Branches, Complaints |
| BO      | Dashboard, Customers, Transactions, Complaints |
| TELLER  | Dashboard, Transactions only |
| AUDITOR | Dashboard, Customers, Transactions, Loans, Risk, Fraud, Financial, Audit Log |

**Action-level permissions** (`ACTIONS`) — what operations a role can perform within accessible modules:

| Role    | Allowed Actions |
|---------|-----------------|
| CMD     | view, create, edit, delete, approve, reject, block, export, report |
| ED      | view, create, edit, approve, reject, block, export, report |
| GM      | view, create, edit, approve, reject, export, report |
| DGM     | view, create, edit, approve, reject, export |
| RM      | view, create, edit, approve |
| BM      | view, create, edit, approve |
| BO      | view, create, edit |
| TELLER  | view, create |
| AUDITOR | view, export |

Branch-scoped roles (BM, BO, TELLER) additionally see only data belonging to their assigned branch.

---

## Modules

### 🏠 Executive Dashboard
Strategic overview visible to all roles, scoped by branch/zone automatically. Displays live KPI cards (deposits, advances, net profit, GNPA ratio), recent transactions feed, open fraud alerts, and Q3 FY25 financial ratios.

### 👥 Customer Management
Full customer lifecycle management. Supports SAVINGS, CURRENT, NRI, and FD account types. Features include:
- Create new customer accounts with auto-generated account numbers (PNB format)
- Full edit form for all customer fields
- Inline editing: click name, mobile, balance, or KYC status directly in the table
- View modal showing full profile, last 5 transactions, and linked loans
- Block / unblock accounts (role-gated)
- Delete customer records (CMD/ED only)
- Search by name, account number, or mobile
- Filter by account type and KYC status
- Paginated display (12 per page)

### 💸 Transaction Ledger
Real-time transaction processing with live balance updates:
- Post new transactions (CREDIT, DEBIT, NEFT, RTGS, IMPS, UPI, ATM)
- Automatic balance deduction/addition on the customer record
- Real-time fraud scoring — transactions above threshold are automatically FLAGGED and a fraud alert is generated
- Inline editing of status and description
- Filter by channel and status
- Channel-wise volume summary
- Paginated display (12 per page)

### 🏦 Loans & Credit
End-to-end loan lifecycle management:
- Apply for loans: HOME, PERSONAL, CAR, EDUCATION, KISAN, MSME, CORPORATE, PROJECT
- Live EMI calculator using the standard reducing balance formula
- Approve / Reject / Disburse actions, each gated to appropriate roles (BM and above)
- Inline editing of interest rate, CIBIL score, and status
- Full edit modal for rate, NPA days, collateral, and status
- NPA tracking with day counter
- Portfolio summary by status
- Interest rate reference grid

### ⚠️ Risk Management
- Four-quadrant risk matrix: Credit, Market, Liquidity, Operational
- Live RBI compliance dashboard: CRR (4%), SLR (18%), PSL (40%), CAR (11.5%), LCR (100%)
- Sector credit exposure table with utilisation progress bars and risk classification
- Quarterly NPA trend from financial snapshots

### 🔍 Fraud Detection
AI-powered fraud alert management:
- Real-time alert feed sorted by severity (CRITICAL → HIGH → MEDIUM → LOW)
- Block account directly from the alert (role-gated to CMD/ED/GM/DGM)
- Mark alerts as Investigating or Resolved
- Inline status editing on all alert rows
- Alert type breakdown with percentage visualisation
- Create manual fraud alerts (BO and above)
- Alert types: VELOCITY, IDENTITY, UNUSUAL_AMOUNT, GEO_ANOMALY, DEVICE

### 📊 Financial Reports
- Full P&L statement for FY 2024-25 (Interest Income → Net PAT)
- Key financial ratios panel with inline editing (ROA, ROE, NIM, GNPA, NNPA, CAR, Cost-to-Income)
- Quarterly performance bar chart (Q1–Q3 FY25)
- Changes to financial snapshot data persist to localStorage immediately

### 💰 Treasury
- Investment portfolio breakdown (G-Sec, SDL, T-Bills, Corporate Bonds, Foreign Securities, Equities)
- Live indicative forex rates (USD, EUR, GBP, JPY, AED, SGD)
- ALM maturity profile
- Open forex positions with hedge ratios

### 🏢 Branch Network
- All 8 seeded branches with zone, city, IFSC code, deposits, advances, and NPA %
- Inline editing of deposits, advances, and NPA % directly in the table
- Computed CD ratio per branch
- Performance classification (GOOD / AVERAGE / POOR)
- BM/BO/TELLER roles see only their own branch

### 📩 Customer Complaints
- Register new complaints with category, priority, and customer linkage
- Inline status editing
- Resolve complaints with mandatory resolution notes
- Edit complaint category, priority, and description
- Priority levels: LOW, NORMAL, HIGH, URGENT
- Categories: ATM Issue, Net Banking, Loan Query, KYC, Fraud, Service, Other

### 📋 MIS Reports
- Generate on-demand reports (8 report types × 4 periods × 3 scope levels)
- Report output pulls live data from the current database state
- Scheduled reports table showing frequency, recipients, and status
- Export Full DB button available for roles with `export` permission

### 🔐 User Management
*(CMD and GM only)*
- Full RBAC matrix displayed showing every role's module access and action permissions
- Add new staff members with role, branch, and zone assignment
- Enable / disable user accounts
- Delete user records (CMD only)
- Inline zone editing

### 🔎 Audit Log
*(CMD, ED, AUDITOR)*
- Immutable chronological record of every action in the system
- Captures: timestamp, employee ID, role, action type, module, record ID, and details
- Capped at 500 most recent entries
- Paginated display (25 per page)
- All logins, logouts, creates, edits, deletes, approvals, and exports are recorded

---

## Database Structure

The database is a plain JavaScript object stored in `localStorage` under the key `pnb_mis_db_v2`. It contains the following tables:

| Table | Description |
|-------|-------------|
| `roles` | 9 role definitions with hierarchy levels |
| `users` | Staff accounts with role and branch assignments |
| `branches` | 8 branches across NORTH, WEST, EAST, SOUTH zones |
| `customers` | Customer accounts with balance, KYC, and type |
| `transactions` | All financial transactions with fraud scores |
| `loans` | Loan applications with EMI, CIBIL, and NPA tracking |
| `fraud_alerts` | AI-generated and manual fraud alerts |
| `financial_snapshots` | Quarterly P&L and ratio data |
| `complaints` | Customer grievance records |
| `audit_log` | Full system activity trail |
| `_seq` | Auto-increment sequence counters per table |

### Seeded Data Summary
- 9 roles, 10 users, 8 branches
- 8 customers across various account types
- 8 sample transactions (RTGS, NEFT, IMPS, UPI, ATM, Branch)
- 7 loans across all loan types
- 4 fraud alerts (3 OPEN, 1 RESOLVED)
- 3 financial snapshots (Q1, Q2, Q3 FY25)
- 4 customer complaints

---

## Exporting and Connecting to an External Database

The **⬇ Export DB** button in the top navigation bar downloads a JSON file named `PNB_MIS_DB_YYYY-MM-DD.json` containing the complete database.

To connect to a real backend:

1. Export the JSON from the browser
2. POST the JSON to your backend API endpoint
3. The backend parses it and inserts records into PostgreSQL, MySQL, or MongoDB
4. When the backend responds with updated data, save it as a `.json` file
5. Use **⬆ Import DB** in the browser to load the updated state

The JSON structure maps directly to relational tables. Example schema for PostgreSQL:

```sql
-- customers table
CREATE TABLE customers (
  id           SERIAL PRIMARY KEY,
  account_no   VARCHAR(20) UNIQUE NOT NULL,
  name         VARCHAR(120) NOT NULL,
  email        VARCHAR(120),
  mobile       VARCHAR(15) NOT NULL,
  kyc          VARCHAR(10) DEFAULT 'PENDING',
  type         VARCHAR(10) DEFAULT 'SAVINGS',
  balance      NUMERIC(15,2) DEFAULT 0,
  branch_id    INTEGER REFERENCES branches(id),
  active       SMALLINT DEFAULT 1,
  created_at   TIMESTAMP DEFAULT NOW()
);
```

---

## MIS Levels Implemented

| Level | Type | Users | Purpose |
|-------|------|-------|---------|
| Strategic | ESS (Executive Support System) | CMD, ED | Long-term planning, P&L, GNPA trend, risk dashboards |
| Tactical | MIS | GM, DGM, RM | Branch performance, loan approvals, zone reports |
| Operational | TPS (Transaction Processing System) | BM, BO, TELLER | Daily transactions, account creation, EMI updates |

---

## Assignment Information

| Field | Detail |
|-------|--------|
| Student Name | Kapoor Prince Dhiraj |
| Eligibility No | 2024000555 |
| Company | Punjab National Bank (PNB) |
| Sector | Banking |
| Type | Public Sector Undertaking (PSU) |
| Subject | Management Information System (MIS) |
|Created By | Atharv Pose |

---

## Browser Compatibility

| Browser | Support |
|---------|---------|
| Chrome 90+ | ✅ Full support |
| Firefox 88+ | ✅ Full support |
| Edge 90+ | ✅ Full support |
| Safari 14+ | ✅ Full support |

localStorage must be enabled (it is by default in all modern browsers). The application works fully offline after the first page load.

---

## Known Limitations

- Passwords are stored using a lightweight djb2 hash for demo purposes. A production deployment would use bcrypt or Argon2 on a secure backend.
- localStorage has a ~5 MB cap per origin. For large datasets, export to a backend database.
- The fraud scoring algorithm is a simplified heuristic (amount-to-limit ratio + channel weight). A production system would use a trained ML model.
- Financial data in the P&L section is static seed data for FY 2024-25; only the ratio fields in the snapshot table are editable inline.
