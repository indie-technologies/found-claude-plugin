---
name: find-transactions
description: Find specific transactions or answer spending questions in the user's Found account. Use when the user asks about a particular charge, payment, deposit, or vendor ("what was that $400 charge", "did the client pay me", "how much did we spend at Amazon", "what did I pay for software last quarter").
---

# Find transactions

## Pick the right view

Found exposes two views of the same money. Choose based on the question:

| Question is about | Use | Why |
|---|---|---|
| Categories, vendors, receipts, tags, "spending on X", anything bookkeeping | `list_bookkeeping_transactions` | Includes categories, tags, notes, receipt status, and imported or manually entered transactions |
| Exactly what hit the bank account, statement lines, transfers between accounts | `list_transactions` | The bank-statement view of real money movement |

Default to `list_bookkeeping_transactions`; it matches what the user sees in the Found app.

## Searching

Neither tool filters by text or amount, so narrow by date and filter the results yourself.

1. Get `business_token` from `list_businesses` (the `id` field) once.
2. Choose the smallest date range that answers the question. Both tools default to the last 30 days; pass `start_date` and `end_date` as `YYYY-MM-DD`.
3. If `total` is more than the rows returned, page with `start` (offset) and `limit` (up to 500) until you have everything in range.
4. Match on `title`, `vendor_name`, `note`, `tags`, and amount. For an amount the user remembers roughly, match within a few dollars and show the candidates.

## Reading amounts correctly

- `list_bookkeeping_transactions`: `amount` is in cents and always positive. `direction` is `"to"` for money in and `"from"` for money out. `amount_dollars` is a display string like `"$1,234.56"`. Categories are in `book_entries[].category_name`, and one activity can be split across several categories.
- `list_transactions`: `amount_dollars` is a plain string like `"-12.34"`; credits are positive and debits negative. Results are oldest first.

## Answering

- For one transaction: date, description or vendor, amount, direction, category, and whether a receipt is still needed.
- For "how much did we spend on X": the total, the number of transactions, and the top few line items. State the date range you used.
- Show at most about 10 rows in the reply. Offer an export (the export-transactions skill) for more.

If the user wants to recategorize, add a receipt, or dispute a charge, explain that the Found connection is read-only and point them to that transaction in the Found app.
