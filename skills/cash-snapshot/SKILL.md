---
name: cash-snapshot
description: Summarize balances across the user's Found businesses and accounts. Use when the user asks how much money they have, what their balance is, how much is in a specific pocket or account, what is pending, or wants a quick cash overview.
---

# Cash snapshot

Answer balance questions in one pass, with the fewest tool calls.

## Steps

1. Call `list_businesses` once per conversation. The token is in the `id` field; reuse it as `business_token` rather than calling `list_businesses` again.
2. If the user asked about a single business, or only has one, call `list_accounts` for that business. For "all my businesses", call `list_accounts` for each.
3. Present, per business:
   - Each open account with its name, last four digits (`display_mask`), and `available_balance_dollars`.
   - Pending amounts only where `pending_balance_dollars` isn't `0.00`.
   - A total of available balances.

## Reading the numbers

- `available_balance_dollars` is what can be spent now. `pending_balance_dollars` is money still settling. `current_balance_dollars` is the two added together.
- Accounts are called pockets inside Found. Use the user's pocket names, and say "account" in prose.
- Archived accounts are hidden by default. Pass `include_archived: true` only if the user asks about closed or archived accounts.
- If an account's `status` isn't `active`, say so plainly and suggest the user check the Found app or contact Found support. Don't speculate about why.

## After answering

Offer one natural next step, not a menu. Examples: "Want to see what came in and went out this month?" or, when there are several accounts, "Want me to reconcile these for last month?"

Don't set up repeated or scheduled balance checks unless the user explicitly asks. If they do, check no more than once a day.
