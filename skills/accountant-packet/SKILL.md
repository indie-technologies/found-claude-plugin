---
name: accountant-packet
description: Put together a quarter-end or year-end package from the user's Found books for their accountant or bookkeeper, including profit and loss, a categorized ledger, open items, and balances. Use when the user is preparing for tax season, year-end, a quarterly review, a meeting with their accountant or CPA, a loan application, or asks for "everything my accountant needs".
---

# Accountant packet

Produce one spreadsheet with a short cover note that an accountant can work from without logging into Found.

## 1. Scope

- Business: from `list_businesses` (token is in the `id` field).
- Period: ask if it isn't clear. The usual choices are last quarter or the last full year.
- Ask whether the accountant wants anything specific. If the user doesn't know, build the standard packet below.

## 2. Gather

Follow the export-transactions skill's fetching rules: one month at a time, `limit: 500`, and page until each month's rows equal its `total`.

1. **Ledger**: `list_bookkeeping_transactions` for the whole period.
2. **Profit and loss**: `ask_assistant` with "What was my profit and loss from <start> to <end>, by month, with income and expenses by category?" Cross-check the totals against the ledger, grouped by `book_entries[].category_name`, excluding `Personal` and `Funding`.
3. **Balances**: `list_accounts` for current balances. For period-end balances, follow the reconcile skill's roll-forward method, and label the results as derived.
4. **Open receivables** (if the business invoices): in the same `ask_assistant` thread, "List invoices created on or before <end date> that are still unpaid, with client, amount, and due date." Invoice status is current, not historical, so label the tab "unpaid as of today".

## 3. Build the spreadsheet

One file, one tab each:

| Tab | Contents |
|---|---|
| Summary | Business name, period, net profit, total income and expenses, account balances, count of open items |
| P&L by month | Income, expense categories, and net profit for each month, plus a total column |
| Ledger | Every transaction with date, title, vendor, signed amount, category, tags, note, source, and receipt status |
| Open items | Uncategorized expenses, transactions still needing receipts, and pending items |
| Receivables | Unpaid invoices at period end, if any |

Amounts are in dollars; convert from cents. Money out is negative on the Ledger tab. Name the file like `acme-accountant-packet-2025.xlsx`.

## 4. Cover note

Draft a few sentences the user can send with the file: the period covered, net profit, anything still open (such as "12 transactions still need receipts"), and a note that the figures come from Found's books. Point out any open items the user should fix first; the month-end-close skill helps with that.

Don't describe the packet as a tax return or tax filing, and don't suggest deductions or filing positions. That's the accountant's job.
