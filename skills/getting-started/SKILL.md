---
name: getting-started
description: Show what Claude can do with the user's Found account and suggest useful next steps based on their data. Use when the user has just connected Found, asks what Found can do in Claude, asks how to use Found here, or asks for ideas about managing their business finances with Found.
---

# Getting started with Found

Give the user a short, personalized tour: a quick look at where their business stands, then two or three workflows that fit what you see. Don't list every capability; pick the ones their data says will help.

## Steps

1. Call `list_businesses` once. It returns each business's token in the `id` field; reuse it as `business_token` for every other tool. If there are several businesses, cover all of them briefly or ask which one to focus on.
2. For the chosen business, call `list_accounts` and `list_bookkeeping_transactions` for the last 30 days (the default).
3. Look for signals in that data:
   - Count activities where `needs_receipt` is true.
   - Count money-out activities whose `book_entries` include a `category_name` of `"Uncategorized"`.
   - Note whether money arrives from Stripe, or from invoice payments.
   - Note how many accounts (pockets) exist and whether money moves between them.
4. Reply with:
   - One or two sentences on the current balance picture (total available across accounts).
   - Two or three suggested next steps, each phrased as something the user can ask, tied to what you saw. Match signals to skills:

| Signal | Suggest |
|---|---|
| Missing receipts or uncategorized items | "Help me close out last month" (month-end close) |
| Many transactions, or the user mentions an accountant | "Put together a packet for my accountant" |
| Stripe payouts or several accounts | "Reconcile my accounts for last month" |
| Invoice payments | "Which invoices are still unpaid?" |
| Always a good option | "What was my profit last month, and how much should I set aside for taxes?" |

5. Mention in one line that the Found connection is read-only: Claude can look things up and prepare work, and changes are made in the Found app.

## Keep it short

This is a first impression. Keep the reply under about 150 words, with no tables of raw transactions. If the user can't do something they want, offer to pass the request to the Found team with `send_feedback`.
