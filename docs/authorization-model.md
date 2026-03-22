# Authorization Model Design

## Scenario

The organization manages sensitive data including payroll, financial reports, and employee records. Unauthorized access could result in financial fraud, data breaches, and compliance violations.

---

## Objective

Design a secure authorization model that ensures:

* Only authorized users can access specific resources
* Access is limited to what is strictly necessary
* Critical actions require separation of responsibilities

---

## Chosen Model: Hybrid (RBAC + ABAC)

A hybrid model combining Role-Based Access Control (RBAC) and Attribute-Based Access Control (ABAC) was selected.

* RBAC provides structured access using predefined roles
* ABAC adds dynamic conditions such as department, time, and clearance

---

## Users

* Employee
* HR Staff
* Finance Analyst
* Manager
* System Administrator

---

## Roles and Attributes

### Roles (RBAC)

* EMPLOYEE
* HR
* FINANCE
* MANAGER
* ADMIN

### Attributes (ABAC)

* Department (HR, Finance, IT)
* Clearance level (Low, Medium, High)
* Employment status (Active / Inactive)
* Time of access (Working hours only)

---

## Protected Resources

* Payroll Database
* Financial Reports
* Employee Records
* Audit Logs

---

## Permissions (Access Matrix)

| Role            | Payroll | Financial Reports | Employee Records | Audit Logs |
| --------------- | ------- | ----------------- | ---------------- | ---------- |
| Employee        | ❌       | ❌                 | Own only         | ❌          |
| HR              | ✅       | ❌                 | ✅                | ❌          |
| Finance Analyst | ❌       | ✅                 | ❌                | ❌          |
| Manager         | ❌       | Read-only         | Limited          | ❌          |
| Admin           | ✅       | ✅                 | ✅                | ✅          |

---

## ABAC Enforcement Logic

Access is granted only if:

* The user is active
* The access occurs during working hours
* The user’s department matches the resource
* The user has the required clearance level

---

## Least Privilege

Each user is granted only the minimum permissions required to perform their job.

Examples:

* Employees can only view their own records
* Finance analysts cannot access payroll data
* HR cannot access financial reports

This reduces the potential impact of compromised accounts.

---

## Separation of Duties

Critical responsibilities are divided:

* HR manages employee records
* Finance handles financial reports
* Admin manages system configurations

No single user has full control over all sensitive operations, reducing the risk of fraud and insider threats.

---

## Simulation (SQL Example)

```sql
CREATE ROLE hr_role;
CREATE ROLE finance_role;

GRANT SELECT, UPDATE ON payroll TO hr_role;
GRANT SELECT ON financial_reports TO finance_role;

GRANT hr_role TO user_hr;
GRANT finance_role TO user_finance;

REVOKE UPDATE ON payroll FROM hr_role;
```

---

## Why This Model Works

This model balances structure and flexibility:

* RBAC simplifies management through roles
* ABAC introduces contextual security
* Enforces least privilege and separation of duties

It reflects real-world IAM systems used in enterprise environments.
