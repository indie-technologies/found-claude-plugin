![Found](assets/icon.svg)

# Found for Claude

Work with your [Found](https://found.com) business banking and bookkeeping in Claude. Check balances, find and export transactions, close out the month, reconcile your accounts, review profit and loss, plan for taxes, chase unpaid invoices, and put together a packet for your accountant.

This plugin pairs the Found connector with skills that teach Claude how to do these jobs well: which data to use, how to page through large histories completely, and what a useful result looks like.

## Get started

1. Add the Found plugin from **Customize > Plugins** in Claude.
2. Open the plugin's **Connectors** tab and connect Found. You'll sign in to your Found account and approve access.
3. Ask something like "What can Found do here?" or "Help me close out last month."

In Claude Code or Cowork you can also run a skill by name, for example `/found:month-end-close`.

## What's included

| Skill | Ask something like |
|---|---|
| `getting-started` | "What can I do with Found here?" |
| `cash-snapshot` | "How much do I have across my accounts?" |
| `find-transactions` | "What was that $400 charge last week?" |
| `export-transactions` | "Export my transactions for Q2 to a spreadsheet." |
| `month-end-close` | "Help me close out March." |
| `reconcile` | "Reconcile my accounts for last month." |
| `profit-loss-and-taxes` | "What was my profit last quarter, and how much should I set aside for taxes?" |
| `unpaid-invoices` | "Which invoices are overdue?" |
| `accountant-packet` | "Put together everything my accountant needs for 2025." |

## Read-only by design

The Found connection can read your businesses, accounts, balances, transactions, bookkeeping, and invoices, and ask the Found Assistant questions about them. It can't move money, pay bills, send invoices, or change your books. When a task needs a change, Claude will tell you what to do in the Found app.

Tax estimates are for planning only and aren't tax advice. Confirm with a tax professional before you file or pay.

## Data

The plugin contains only instructions for Claude and a reference to Found's connector at `https://my.found.com/api/mcp`. It stores nothing and runs no code of its own.

When you use it, Claude requests your account data from Found through that connector, using the access you approved when you connected. Files Claude creates, such as exports, are saved where you're working with Claude. If you use the feedback option, your message is sent to the Found team.

You can disconnect Found at any time from the plugin's **Connectors** tab.

## Privacy

Found's handling of your data is described in the [Found Privacy Policy](https://found.com/legal/privacy).

## Support

Questions or problems: ask Claude to "send feedback to Found", or contact Found support at [found.com](https://found.com).

## License

MIT
