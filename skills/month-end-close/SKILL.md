---
name: month-end-close
description: Review a month of the user's Found bookkeeping and produce a checklist of what to fix before the books are closed. Use when the user wants to close the month, clean up their books, catch up on bookkeeping, find uncategorized transactions or missing receipts, or get ready for their accountant.
---

# Month-end close

Turn a month of bookkeeping into a short, prioritized to-do list. The Found connection is read-only, so the output is a checklist the user works through in the Found app. Don't claim to have fixed anything.

## 1. Scope

- Business: from `list_businesses` (token is in the `id` field).
- Month: default to the last full calendar month. Fetch with `list_bookkeeping_transactions` using that month's first and last day, `limit: 500`, and page with `start` until you have `total` rows.
- Also fetch the prior three months, which gives you a baseline for "unusual".

## 2. Checks

Run these in order and keep the findings for each:

1. **Uncategorized**: money-out activities (`direction: "from"`) where any `book_entries[].category_name` is `"Uncategorized"`. Skip `pending` or `canceled` ones.
2. **Missing receipts**: `needs_receipt: true`.
3. **Possible duplicates**: same amount and same vendor or title within three days. Say "possible"; recurring charges and split payments are often legitimate.
4. **Personal or funding**: entries categorized `Personal` (money out) or `Funding` (money in). List them so the user can confirm they're intended; they're excluded from profit and loss.
5. **Unusual**: vendors seen for the first time this month that cost more than about $500, and categories more than twice their three-month average. Frame these as "worth a look", not as errors.
6. **Still settling**: `pending: true` or `under_review: true`. These may change after the month closes.

Amounts: `amount` is in cents and always positive, so use `direction` for sign.

## 3. Output

Open with a one-line status, for example "March is close: 7 items need attention, about 20 minutes of work."

Then a checklist grouped by check, largest dollar amounts first. For each item: date, title or vendor, amount, and what to do (such as "add a category", "attach receipt", "confirm this isn't a duplicate"). Cap each group at 10 items and give the count of the rest. Skip empty groups, and say so in one line at the end ("No duplicates or unusual charges found.").

If there are more than about 25 items, offer the list as a spreadsheet instead of inline.

Close with how to act on it: categories and receipts are updated on each transaction in the Found app, and receipts can be photographed and uploaded there.

## 4. Next step

Offer one: the reconcile skill ("Want me to check the books against the bank for March?"), then profit-loss-and-taxes once the checklist is clear.
