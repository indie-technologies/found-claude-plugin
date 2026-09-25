# Maintaining this plugin

The skills name the Found MCP server's tools and fields directly, so they have to change together with the server. The server lives in Found's main repository under `api-backend/site/packs/mcp_server`.

## When the server changes

- **A tool is renamed, removed, or changes its arguments or response fields**: update every skill that mentions it, then raise `version` in `.claude-plugin/plugin.json`. Search the skills for the tool name.
- **A new tool is added**: new tools reach users without a plugin release, but check whether a skill should use it instead of `ask_assistant`. Invoices and profit and loss currently go through `ask_assistant`, so a dedicated tool for either should replace that path in the skills.
- **The server gains write tools**: every skill and the README promise read-only access. Revisit those statements before a write tool ships.

## Facts the skills rely on

| Fact | Used by |
|---|---|
| `list_businesses` returns the business token as `id` | all skills |
| `list_transactions`: oldest first, default and max 500 rows, `amount_dollars` is a signed plain string | find, export, reconcile |
| `list_bookkeeping_transactions`: newest first, default 200 and max 500 rows, `amount` is positive cents plus `direction` (`to` or `from`), categories in `book_entries[].category_name` | most skills |
| Both transaction tools default to the last 30 days and page with `start`, `limit`, and `total` | export, close, reconcile, packet |
| `ask_assistant` reaches profit and loss, invoice search and details, client lookup, and Stripe payouts; `thread_token` continues a conversation | P&L, invoices, reconcile, packet |
| `"Uncategorized"` appears only on settled money-out activity | close, P&L |
| `list_accounts` marks the Tax account with `pocket_type: "tax"`; the assistant can report Found's estimated taxes owed and whether auto withholding is on | P&L and taxes |

## Releasing

1. Edit the skills and raise `version`.
2. Run `claude plugin validate .` from the repository root.
3. Test in Claude Code with `claude --plugin-dir .`, and in claude.ai by uploading a zip under **Customize > Plugins > Add > Upload plugin**.
4. Have customer-facing wording changes reviewed before merging, as with other Found customer copy.
5. Merge to `main`. Once listed, the directory picks up each merge to the tracked branch.
