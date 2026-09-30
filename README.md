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
key to paste, here or anywhere else. You can also start without an account:
`search_companies` and `sp_health` are free, and an unconnected caller still gets
**3 research answers and 50 data lookups per rolling 30 days**. Connecting an
account raises the allowance.

## What you can ask

- *"What were NVDA's revenue and free cash flow in its latest filed year?"*
- *"How has AAPL's share count moved over the filings you can see?"*
- *"Compare MSFT and GOOGL on filed margins — keep the periods separate."*
- *"What has TSLA filed in the last few months?"*
- *"What did management say about margins in the latest 10-Q?"*

## The tools

| Tool | What it returns |
| --- | --- |
| `search_companies` | A company name or ticker resolved to one US-listed company, with its SEC CIK. Free. |
| `get_company_fundamentals` | Filed figures by fiscal period — revenue, gross profit, operating income, net income, diluted EPS, free cash flow, cash, debt and shares outstanding |
| `search_filings` | The 10-K / 10-Q / 8-K / 20-F / proxy timeline, by form and date, with EDGAR links |
| `get_filing` | The text of one filing — a named section such as Risk Factors or MD&A, or a search within it |
| `ask_filings` | A filing-grounded written answer, with the pulls behind it, so the figures can be checked. Metered — it costs far more than a lookup. |
| `sp_health` | Server version and tool count — the connection check. Free. |

The skill is the part that matters day to day: it holds Claude to the period, the
citation, and the missing value. Without it a model will happily smooth a `null` into
a number.

## Coverage — read this before relying on it

**US-listed companies that report in USD to the SEC.** No international listings, no
analyst estimates, no forward data. A figure the company did not file is returned as
`null` and is never estimated or annualised.

That limit is stated up front because a tool that quietly returns nothing for a
company it does not cover is worse than one that says so.

Fund data — ETF and mutual-fund costs, holdings and allocation — is deliberately not
part of this connector: it is third-party fund data, not SEC company data. It is
available on the [REST API](https://www.stockportfolio.pro/docs/api-mcp) instead.

## Links

- Setup and pricing for every client: <https://www.stockportfolio.pro/docs/api-mcp>
- Support: support@stockportfolio.pro

## License

MIT — see [LICENSE](LICENSE).
