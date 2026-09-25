---
description: Produce a filing-grounded brief on one US-listed company — filed figures, what changed, and what the company has filed recently
---

# Filed brief

Produce a brief on the US-listed company the user named (ticker: $ARGUMENTS).

Use the StockPortfolio tools. Do not answer from your own knowledge, and do not
estimate a figure the filings do not contain.

1. `sp_financials` for the ticker — period-locked revenue, net income, operating and
   free cash flow, share count, margins.
2. `sp_filing` — what the company has filed recently, with EDGAR links.
3. If the numbers suggest a capital-allocation story (a rising share count, or cash
   flow diverging from net income), pull the matching `sp_financials` view —
   `dilution`, `buybacks-vs-dilution`, or `cash-flow-quality` — rather than
   speculating about the cause.

Write it up as:

- **Filed figures** — each with its fiscal period and the filing it came from.
- **What changed** — only comparisons between the same period type. State both
  periods. If the filings do not support a comparison, say that instead.
- **Recent filings** — the timeline, with links.
- **Not disclosed** — anything the user would reasonably expect that the filings do
  not contain. A `null` goes here; it is a finding, not a gap to paper over.

Keep it short and sourced. Every number carries its period and its filing. Close by
naming the coverage limit if the user's question reached past it: US-listed,
USD-reporting companies only, no estimates and no forward data.
