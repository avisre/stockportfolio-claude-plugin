# StockPortfolio.pro — Verified Financial Data, for Claude

Ask Claude about a US company's reported numbers and get the filing they came from.

This plugin connects Claude to the [StockPortfolio.pro](https://www.stockportfolio.pro)
MCP server and adds a skill that teaches it how to use the data honestly: every
figure keeps its fiscal period and the SEC filing it was read from, and a figure the
company has not filed comes back as missing rather than as a plausible estimate.

## Install

Add the marketplace once, then install the plugin:

```
claude plugin marketplace add avisre/stockportfolio-claude-plugin
claude plugin install stockportfolio@stockportfolio
```

Or both in one step from inside a session:

```
/plugin install stockportfolio --marketplace avisre/stockportfolio-claude-plugin
```

To try it without installing anything, load it straight from a checkout:

```
claude --plugin-dir /path/to/stockportfolio-claude-plugin
```

On first use Claude opens a sign-in window to connect the server — there is no API
key to paste. Connecting gives you the free allowance (2 questions and 50 data
lookups per 30 days); a paid plan raises it.

## What you can ask

- *"What were NVDA's revenue and free cash flow in its latest filed year?"*
- *"How has AAPL's share count moved over the filings you can see?"*
- *"Compare MSFT and GOOGL on filed margins — keep the periods separate."*
- *"Rank these ten tickers by filed revenue growth."*
- *"What has TSLA filed in the last few months?"*
- *"What does VOO charge, and what does it hold?"*

## The tools

| Tool | What it returns |
| --- | --- |
| `sp_financials` | Period-locked filed figures, plus views for earnings quality, dilution, buybacks, cash-flow quality and debt |
| `sp_filing` | The 10-K / 10-Q / 8-K / Form 4 timeline, with EDGAR links |
| `sp_compare` | Two companies side by side, periods kept separate |
| `sp_screen` | A watchlist ranked by filed revenue growth |
| `sp_fund` | ETF and mutual-fund costs, holdings, allocation, performance, risk |
| `sp_ask` | A filing-grounded written answer, labelled with its source class |
| `sp_health` | Server version and the allowance you have left |

The skill is the part that matters day to day: it holds Claude to the period, the
citation, and the missing value. Without it a model will happily smooth a `null` into
a number.

## Coverage — read this before relying on it

**US-listed companies that report in USD to the SEC.** No international listings, no
analyst estimates, no forward data. Fund data (`sp_fund`) is third-party fund data,
not SEC company data, and is not redistributable.

That limit is stated up front because a tool that quietly returns nothing for a
company it does not cover is worse than one that says so.

## Links

- Setup and pricing for every client: <https://www.stockportfolio.pro/docs/api-mcp>
- Prefer a local server? `npx -y stockportfolio-mcp` bridges the same endpoint over
  stdio: <https://www.npmjs.com/package/stockportfolio-mcp>
- Support: support@stockportfolio.pro

## License

MIT — see [LICENSE](LICENSE).
