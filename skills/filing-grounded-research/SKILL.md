---
name: filing-grounded-research
description: Answer questions about a specific US-listed company's filed financials using SEC filings — revenue, net income, operating and free cash flow, margins, share counts, debt, and the 10-K/10-Q/8-K/20-F timeline. Use when the user asks about a company's reported numbers, wants two companies compared, asks which filing reports a figure, or asks what a company has filed recently. Not for share prices, forecasts, analyst estimates, or non-US companies.
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

4. **Prefer the deterministic tools for numbers.** `get_company_fundamentals`,
   `search_filings` and `get_filing` read filed values and filing text directly.
   `ask_filings` is an AI answer and is right for a question that needs reasoning over
   filing content or spans several filings — it is metered and costs far more, so
   reach for it when the question genuinely needs it, not by default.

## Which tool

| Question | Tool |
| --- | --- |
| Which company is this? a name with no ticker, or an ambiguous one | `search_companies` |
| One company's filed figures, by fiscal period | `get_company_fundamentals` |
| What has this company filed lately, with EDGAR links | `search_filings` |
| What a filing actually says — Risk Factors, MD&A, Legal Proceedings, or a term | `get_filing` |
| A question needing reasoning across filing content, or prose | `ask_filings` |
| Is the server up, what version, how much allowance is left | `sp_health` |

Two pairs are easy to confuse, and the tool descriptions say which is which:

- **`search_filings` is metadata, not text.** It returns the form, the filing date,
  the accession number and a URL. To read what is inside the filing, call
  `get_filing` with the accession number it gave you.
- **`get_company_fundamentals` is the number; `get_filing` is the sentence.** Use the
  first for a reported figure, the second for what management said about it.

## Coverage — say the limit out loud

The server covers **US-listed companies that report in USD to the SEC**. It does not
cover international listings, it has no analyst estimates, and it has no forward
data. When a user asks about a non-US company, say so plainly and stop — do not
substitute a US peer as though it were the same company, and do not answer from
memory instead. Naming the limit is more useful than a plausible wrong number.

Fund data — ETF and mutual-fund costs, holdings and allocation — is not reachable
through this server. If the user asks for it, say that this connector returns SEC
company filings only.

## Working with the answer

- An `ask_filings` answer returns the pulls behind it. Check them, and cite the filing
  the figure came from as you would for any other tool.
- A `null` and an empty result are different: `null` means the company filed nothing
  for that line, an empty list means the search matched nothing. Say which one you got.
- Check `sp_health` if a call fails with a quota error; it answers even when the
  allowance is spent and reports what is left. It is also the free way to confirm the
  connection works.
- If a tool returns an error naming a bad ticker or a missing argument, fix the
  argument and retry once. Do not fall back to answering from your own knowledge —
  that is the failure mode this toolset exists to prevent.
