# OrgExpenses

[![Tests](https://github.com/ScintillaDark/orgexpenses/actions/workflows/tests.yml/badge.svg)](https://github.com/ScintillaDark/orgexpenses/actions/workflows/tests.yml)
[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

OrgExpenses is a Laravel expense-management application for recording employee
expenses, routing them through a fixed approval process, and tracking
reimbursements and related balances.

> **Project status:** This repository is a development/demo project. Tenant
> isolation, enforced maker-checker separation, and configurable approval
> workflows are product goals, but are **not implemented** in the current code.
> Review the limitations below before using real financial or employee data.

## What is implemented

- Email one-time-passcode (OTP) sign-in.
- User, role, and permission management building blocks, with Gate checks for
  selected administrative routes. Authorization coverage is not complete.
- Projects with a reporting-manager association.
- Expense records for advances, part payments, and complete payments, with
  optional links between related claims.
- Expense modes for cash, company card, personal card, personal UPI, and petty
  cash; invoice and payment attachments are supported.
- A fixed pending/approved/rejected expense status flow, a manager-oriented
  pending-approval dashboard, and payment records for approved expenses.
- Per-user wallets and transaction history, plus company-card record management.
- INR conversion support. Non-INR conversion depends on configured currency
  records and an exchange-rate API key.

## Product direction (not implemented)

The following are planned product capabilities, not features of the current
code:

- Multi-tenant organization support with tenant-scoped data and isolation.
- End-to-end maker-checker enforcement.
- Configurable, multi-step approval workflows.

The current approval flow is fixed in application code. Approval endpoints do
not yet enforce a separate checker policy, and the repository does not contain
tenant-scoping models or migrations. Do not treat this version as a
multi-tenant or production-ready financial system.

## Application structure

| Path | Responsibility |
| --- | --- |
| `app/Http/Controllers/` | Authentication, expenses, projects, payments, wallets, cards, and administration |
| `app/Models/` | Eloquent models and relationships |
| `database/migrations/` | Relational schema |
| `resources/views/` | Blade user interface |
| `routes/web.php` | Browser routes |
| `routes/api.php` | API route definitions |
| `tests/` | PHPUnit test suites |

The application uses Laravel 8, Blade, Eloquent, MySQL-compatible storage,
Laravel Sanctum, and Laravel Mix for front-end assets.

## Getting started

See [INSTALL.md](INSTALL.md) for prerequisites, environment setup, database
migrations, first-user bootstrap, and local development commands.

## Testing

```bash
php artisan test
```

The current test suite is still mostly Laravel scaffold coverage; domain-level
tests for expenses, authorization, approvals, and payments are needed.

## Security and production use

Before using this application with real data, review and strengthen
authorization, tenant isolation, approval separation, OTP delivery and
throttling, file handling, and test coverage. See [SECURITY.md](SECURITY.md)
for vulnerability reporting. Keep credentials in `.env`; never commit them.

## Contributing

Bug reports and focused pull requests are welcome. Read
[CONTRIBUTING.md](CONTRIBUTING.md) before contributing.

## License

OrgExpenses is distributed under the [MIT License](LICENSE).
