---
name: unpaid-invoices
description: Review the user's unpaid and overdue Found invoices, age them, and draft follow-up messages to clients. Use when the user asks who owes them money, which invoices are unpaid or overdue, about accounts receivable, whether a client has paid, or wants help chasing late payments.
---

# Unpaid invoices

Found invoices are available through the Found Assistant, so every lookup here goes through `ask_assistant`. Start one thread and reuse its `thread_token` for follow-ups so the assistant keeps context.

## 1. Pull the list

Get `business_token` from `list_businesses` (the `id` field), then call `ask_assistant` with a question like: "List all my unpaid invoices, including overdue ones. For each, give the invoice number, client name and email, amount, created date, due date, and whether it's overdue."

For one client, name them: "List unpaid invoices for Acme Co." If the assistant says it found several matching clients, ask the user which one.

For details on one invoice, such as line items or reminders already sent, ask about that invoice number in the same thread.

## 2. Age and prioritize

Group by days past the due date: not yet due, 1–30, 31–60, 61–90, and over 90. Invoices with no due date go in their own group. Show the total in each group and the overall amount outstanding.

Note what the invoice data means:
- An invoice the client marked as paid isn't confirmed; the payment may not have arrived yet. Check the bank activity with `list_transactions` if it matters.
- An invoice total isn't necessarily the outstanding balance if there were partial payments. Say so when it's relevant.

## 3. Draft follow-ups

For overdue invoices, offer to draft a short, polite reminder for each client, getting firmer with age. Include the invoice number, amount, the original due date, and a line inviting the client to reply if there's a problem. Match the user's tone if they've given you an example.

Drafts only: the user sends them. Mention that Found can also send automatic invoice reminders, which the user can set up on the invoice in the Found app.

## 4. Next step

Offer to check whether any payments recently arrived for these invoices (the find-transactions skill), or to include receivables in an accountant packet.
