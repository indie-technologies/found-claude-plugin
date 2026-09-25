---
name: profit-loss-and-taxes
description: Summarize the user's profit and loss from their Found books and help them think about how much to set aside for taxes. Use when the user asks about profit, income, expenses, margins, top spending categories, how the business is doing, month-over-month or year-over-year trends, or how much to save for taxes or quarterly estimated taxes.
---

# Profit and loss, and tax set-aside

## Profit and loss

1. Get `business_token` from `list_businesses` (the `id` field).
2. Settle the period. "Last month" means the previous calendar month. For trends, use at least three months, or the same period last year for year-over-year.
3. Ask the Found Assistant, which calculates profit and loss from the user's books with a monthly breakdown: call `ask_assistant` with the `business_token` and one precise question covering the whole period, such as "What was my profit and loss from 2026-01-01 to 2026-03-31, by month, with income and expenses broken down by category?" Ask for the full range at once rather than one month at a time. For follow-ups, pass the `thread_token` from the previous response.
4. For a category-level breakdown, or to double-check the assistant's answer, sum `list_bookkeeping_transactions` for the period by `book_entries[].category_name`. Use each book entry's `amount` (cents), and exclude `Personal` and `Funding`, which aren't business income or expense.
5. Present income, expenses, and net profit for each period, the top five expense categories with their change from the prior period, and one or two observations worth acting on ("Software costs doubled since January, mostly from two new subscriptions").

Uncategorized expenses make profit and loss unreliable. If they're more than about 5% of expenses, say so, and suggest running the month-end-close skill first.

## Taxes

This is an estimate to help the user plan, not tax advice. Say that once, plainly, and recommend confirming with a tax professional before filing or paying.

1. **Start with what Found already tracks.** Call `list_accounts` and look for an account with `pocket_type: "tax"`. Its balance is what the user has already set aside. Then call `ask_assistant` with "What are my estimated taxes owed, and is automatic tax withholding turned on?" Found estimates taxes from the user's income and tax profile, and the assistant can report that estimate and the withholding setting.
2. **Compare.** Show what's set aside next to Found's estimate. If the set-aside is short, say by how much, and mention that the user can adjust how much of each deposit goes to the Tax account in the Found app.
3. **If there's no Tax account or estimate**, and the user wants their own figure, use net profit for the period and a rate the user provides. If they don't have one, show what a few rates would mean (for example 25%, 30%, and 35% of net profit), and explain that the right rate depends on their tax bracket, state, and self-employment tax.
4. **For quarterly estimated taxes**, summarize net profit for each quarter so far. Found supports paying quarterly estimated taxes in the app for some users; point them there rather than describing payment steps. Don't state IRS due dates or penalty rules as fact; tell the user to check current IRS guidance or ask their tax professional.

Never tell the user what they owe, and don't recommend a filing position.

## Next step

Offer the accountant-packet skill when the user is preparing for filing or a meeting with their accountant.
