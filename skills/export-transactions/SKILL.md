---
name: export-transactions
description: Export the user's Found transactions for a date range to a spreadsheet or CSV. Use when the user asks to export, download, pull, or dump transactions, get a transaction history, or build a ledger for a month, quarter, or year.
---

# Export transactions

Build a complete, correct export and prove it's complete.

## 1. Settle the scope

Ask only for what's missing:
- **Business**: from `list_businesses` (token is in the `id` field).
- **Date range**: default to last calendar month if the user doesn't say.
- **Which view**:
  - **Bookkeeping** (default): `list_bookkeeping_transactions`. Everything in the user's books, including imported and manually entered transactions, with categories, tags, notes, and receipt status. Best for accountants, taxes, and profit and loss.
  - **Bank statement**: `list_transactions`. Only money that actually moved on Found accounts, including transfers between accounts. Best for reconciliation.

## 2. Fetch everything

- Fetch one calendar month at a time, using `start_date` and `end_date` (`YYYY-MM-DD`, both inclusive).
- Within each month, request `limit: 500` and page with `start` (0, 500, 1000, and so on) until you've collected `total` rows.
- After each month, check that the rows you collected equal that month's `total`. If they don't, refetch that month before moving on.
- Don't restart from the beginning to "double-check"; the per-month totals are the check.

## 3. Build the file

Create a CSV, or a spreadsheet if the user prefers one, with one row per transaction.

Bookkeeping columns: `date`, `title`, `vendor_name`, `direction` (money in or money out), `amount` (converted from cents to dollars), a signed amount (negative for money out), `category` (join multiple `book_entries[].category_name` values with "; "), `tags`, `note`, `needs_receipt`, `source` (`found`, `external_import`, or `manual`), `activity_token`.

Bank statement columns: `transacted_at`, `settled_at`, `description`, `counterparty_name`, `amount_dollars` (already signed), `method`, `type`, `pending`, `account_token`, `transaction_token`.

Sort by date. Name the file after the business and range, such as `acme-bookkeeping-2026-01-01-to-2026-06-30.csv`.

## 4. Report back

- Row count per month and in total, plus total money in and total money out.
- Anything notable: pending items, rows marked `needs_receipt`, uncategorized money out.
- Where the file is saved, or attach it.

Then offer one next step that fits: the accountant-packet skill for a year or quarter, the month-end-close skill if many rows are uncategorized, or the reconcile skill for a bank-statement export.

## Large ranges

A full year can be thousands of rows. In chat, say roughly how many rows to expect before fetching (use the first month's `total` as a guide), and keep row data out of the reply itself; put it in the file.
