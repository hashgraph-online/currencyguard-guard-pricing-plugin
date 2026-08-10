# CurrencyGuard Guard Pricing

CurrencyGuard Guard Pricing connects Codex to CurrencyGuard's public Streamable HTTP MCP. It helps PSP, treasury, and payments teams inspect indicative FX rates, price invoice-linked Guard protection, resolve settlement dates, model historical best/worst outcomes, and explain the product without executing a trade or making a financial recommendation.

## Connection

The plugin uses the hosted MCP endpoint:

```text
https://api.currencyguard.com/mcp
```

The server registry metadata identifies the transport as Streamable HTTP and authentication as not required. The bundled [`.mcp.json`](./.mcp.json) uses Codex's `type: "http"` representation for that transport. Re-check the live tool list before relying on names or schemas.

## Tool surface

The source registry currently advertises eight read-only or indicative tools:

`price_guard`, `price_guard_extension`, `get_spot_rate`, `get_forward_rate`, `get_historic_best_worst`, `resolve_settlement_date`, `list_supported_currencies`, and `explain_guard_product`.

The source repository's [server.json](https://github.com/rudialexis/currencyguard-mcp/blob/main/server.json) is the metadata source for endpoint, registry id, publisher, and tool descriptions. Live MCP schemas can change; use MCP discovery and `tools/list` as runtime truth.

## Safe workflow

1. Clarify the invoice-linked exposure, currencies, amount, due date or tenor, and desired analysis.
2. Check supported pairs, then retrieve indicative spot/forward context.
3. Price a Guard only with user-supplied exposure details; preserve the returned quote and caveats.
4. Resolve settlement dates and add historical best/worst context when useful.
5. Explain the output as indicative analysis for PSP/treasury review, not advice or an executed transaction.

The bundled [`currencyguard-guard-pricing` skill](./skills/currencyguard-guard-pricing/SKILL.md) encodes these guardrails.

## Important limitations

- This plugin does not execute trades, confirm or settle Guards, convert currency, transfer funds, or access payment accounts.
- It does not determine suitability, eligibility, credit, investment, tax, legal, or regulatory outcomes.
- Rates, prices, fees, and historic scenarios are time-sensitive and should be re-queried before a decision.
- Keep private invoice data and credentials out of tool arguments unless explicitly required and authorized.

## Source and license

The plugin wraps [rudialexis/currencyguard-mcp](https://github.com/rudialexis/currencyguard-mcp) and does not vendor or execute a local server checkout. The source repository is currently owned by `rudialexis`; this temporary Codex packaging repo is staged under `hashgraph-online` pending maintainer transfer.

MIT. See [`LICENSE`](./LICENSE).
