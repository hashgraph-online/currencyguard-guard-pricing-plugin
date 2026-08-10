---
name: currencyguard-guard-pricing
description: Use CurrencyGuard's public MCP to price and explain invoice-linked FX protection, inspect rates, and resolve settlement dates without executing trades or giving financial advice.
license: MIT
---

# CurrencyGuard Guard Pricing

Use the bundled `currencyguard` MCP server for indicative FX protection analysis. Refresh the live MCP tool list when a tool is missing, an input shape is uncertain, or a result looks stale.

## Operating sequence

1. Clarify the invoice or payable/receivable context, currencies, amount, due date or tenor, settlement assumptions, and whether the user wants a quote, rate, stress view, date resolution, or product explanation.
2. Use `list_supported_currencies` before assuming a pair is supported. Use `get_spot_rate` for an indicative current rate and `get_forward_rate` for an indicative tenor-based rate.
3. Use `price_guard` only to return an indicative protection price for the supplied invoice-linked exposure. Preserve the returned fee, rate, currency, amount, tenor, and caveats exactly.
4. Use `price_guard_extension` only when an existing Guard reference and a later settlement date are explicitly supplied. Do not invent a Guard id, invoice, payment, or extension terms.
5. Use `resolve_settlement_date` to make business-date assumptions explicit. Use `get_historic_best_worst` for stress context, not as a prediction or guarantee.
6. Use `explain_guard_product` for product structure and FAQs, then distinguish source facts from general explanation.

## Response quality and financial safety

- Label all rates, prices, fees, and historical scenarios as indicative unless the tool explicitly says otherwise.
- State the quote timestamp, currency pair, notional amount, tenor, settlement date, and data gaps when returned by the server.
- Explain that the Guard is invoice-linked and payment-contingent; do not describe it as a vanilla option, derivative, or unconditional forward.
- Never claim that the plugin executes, confirms, settles, hedges, converts currency, approves eligibility, or provides investment, tax, legal, or regulatory advice.
- Do not recommend a financial product or tell a user to enter a transaction. Present returned figures for review by the user's regulated PSP, treasury, or qualified adviser.
- Keep private invoice identifiers, customer data, payment credentials, and API keys out of tool arguments unless the user has explicitly authorized the minimum necessary data.

## Failure handling

If a call fails, check the current MCP discovery and schema before retrying. Explain whether the issue is an unsupported pair, invalid amount/tenor/date, unavailable quote, or upstream failure. Never fill missing rates or prices with guesses or stale results.
