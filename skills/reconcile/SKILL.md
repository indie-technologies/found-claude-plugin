---
name: reconcile
description: Reconcile the user's Found accounts for a period by checking the books against the bank activity, rolling balances forward, and matching Stripe payouts to deposits. Use when the user asks to reconcile, balance the books, check that numbers match, figure out why a balance looks wrong, or match payouts or deposits.
---

# Reconcile

Show that the books, the bank activity, and the balances agree, and explain every difference you find. Work one business and one period at a time; default to last calendar month.

## Fetch

- `list_businesses` (token is in the `id` field), then `list_accounts` for the business's current balances.
- `list_transactions` for the period (the bank view). It's oldest first, max 500 per page; page with `start` until you have `total`.
- `list_bookkeeping_transactions` for the same period (the books view), paged the same way.
- `list_transactions` from the day after the period ends through today, for the balance roll-back below.

## Check 1: books vs. bank

Only bookkeeping rows with `source: "found"` should appear in the bank view. Rows with `external_import` or `manual` come from outside Found and are expected to be missing from it.

1. Sum the `found` bookkeeping rows as money in and money out. Bookkeeping `amount` is positive cents; `direction` `"to"` is in, `"from"` is out.
2. Sum the bank rows, excluding transfers between accounts (`method: "pocket_transfer"`), which net to zero across the business. Bank `amount_dollars` is already signed.
3. If the totals differ, find the rows responsible: match on date (within two days) and amount, and list what's left over on each side. Pending and canceled items are the usual cause, so call those out first.

## Check 2: balance roll-forward

The tools don't return historical balances, so derive the period-end balance per account:

- Period-end balance = `current_balance_dollars` from `list_accounts`, minus the net of every bank row for that account (`account_token`) dated after the period.
- Period-start balance = period-end balance, minus the net of the period's rows for that account.

Present start, money in, money out, and end for each account. Say plainly that these are derived from activity, and suggest the user compare them with their monthly statement in the Found app. If they share the statement's figures, compare and explain any gap.

## Check 3: Stripe payouts (when the business uses Stripe)

1. Call `ask_assistant` with the `business_token` and a question such as "List my Stripe payouts between 2026-03-01 and 2026-03-31 with amount, status, and arrival date." It returns up to 25 payouts, so split long periods.
2. Match each payout to a bank credit of the same amount within about three days of its arrival date.
3. List payouts with no matching deposit (and their status), and Stripe-looking deposits with no payout.

## Output

A short summary first ("March reconciles, except for 2 pending card charges totaling $84.10"), then one section per check with only the differences, not every matched row. If everything matches, say so and stop.

Differences you can't explain are worth raising with Found support. Offer `send_feedback` if the user thinks the data itself is wrong.

Next step to offer: the accountant-packet skill for the quarter or year.
