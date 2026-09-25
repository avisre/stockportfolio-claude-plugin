---
name: filing-grounded-research
description: Answer questions about a specific US-listed company's filed financials using SEC filings — revenue, net income, operating and free cash flow, margins, share counts, dilution, buybacks, debt, and the 10-K/10-Q/8-K/Form 4 timeline. Use when the user asks about a company's reported numbers, wants two companies compared, asks to rank a watchlist by filed figures, or asks what a company has filed recently. Also use for ETF and mutual-fund cost, holdings and allocation questions. Not for share prices, forecasts, analyst estimates, or non-US companies.
---

# Filing-grounded research

The MCP server behind this skill exposes figures that came out of SEC filings. Every
value carries the fiscal period it belongs to and the filing URL it was read from.
The whole point of the toolset is that you never have to guess a number — so do not
guess one.

## The four rules

1. **Period-lock every figure you report.** A revenue number with no period is not an
   answer. Say "FY2024 revenue of $X, per the 10-K filed 2025-02-14", not "revenue is
   about $X". If two figures cover different periods, keep them separate — never
   compute a growth rate across two different fiscal years and present it as growth.

2. **Carry the filing through to the user.** Each response names the filing it came
   from. Keep the source URL in your answer, at least once per figure you rely on. A
   user who can open the filing can check you.

3. **A `null` is a real answer.** Where a company has not filed a figure, the tool
   returns `null` rather than interpolating. Report the gap. Do not fill it from
   memory, from the share price, or from a prior year. "Not disclosed in the FY2024
   10-K" is the correct answer and it is a useful one.

4. **Prefer the deterministic tools for numbers.** `sp_financials`, `sp_filing`,
   `sp_compare` and `sp_screen` read filed values directly. `sp_ask` is an AI answer
   and is right for a question that spans sources or needs prose — it costs far more
   credits, so reach for it when the question genuinely needs it, not by default.

## Which tool

| Question | Tool |
| --- | --- |
| One company's filed figures, and views like earnings quality, dilution, cash-flow quality, debt | `sp_financials` |
| What has this company filed lately, with EDGAR links | `sp_filing` |
| Two companies side by side | `sp_compare` |
| Rank a watchlist by filed revenue growth (up to 10) | `sp_screen` |
| ETF or mutual fund costs, holdings, allocation, performance, risk | `sp_fund` |
| A question needing prose, or spanning filings and other sources | `sp_ask` |
| Is the server up, what version, how much allowance is left | `sp_health` |

## Fund data is not filing data

`sp_fund` returns third-party fund data — costs, holdings, allocation, performance,
risk. It is not SEC company data and it is not redistributable. Never present a
`sp_fund` figure as though it came from a filing, and label it as fund data when you
report it. A fund and a company are different instruments; do not blend their
metrics into one comparison table without saying which column is which.

## Coverage — say the limit out loud

The server covers **US-listed companies that report in USD to the SEC**. It does not
cover international listings, it has no analyst estimates, and it has no forward
data. When a user asks about a non-US company, say so plainly and stop — do not
substitute a US peer as though it were the same company, and do not answer from
memory instead. Naming the limit is more useful than a plausible wrong number.

## Working with the answer

- The `source` class on an `sp_ask` answer tells you where it came from: `filed`,
  `fund-data`, or `live-web`. Do not present a `live-web` answer as though it were
  filed — the user is making a decision on it.
- Check `sp_health` if a call fails with a quota error; it answers even when the
  allowance is spent and reports what is left.
- If a tool returns an error naming a bad ticker or a missing argument, fix the
  argument and retry once. Do not fall back to answering from your own knowledge —
  that is the failure mode this toolset exists to prevent.
