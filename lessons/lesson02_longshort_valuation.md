# Lesson 2 — Fundamental and Valuation-Driven Strategies (Long/Short Equity)

The main focus of todays lesson is to define the long/short equity strategy, its instruments (stocks, indices, ETFs), and the two valuation toolkits used to generate signals — relative valuation (multiples) and intrinsic valuation (DCF). The emphasis is on how a fund turns a valuation view into a position, and where the process breaks in practice.

---

## 1. Long/short equity: the strategy

The oldest hedge fund strategy (Jones, 1949). Hold long the securities judged cheap, short those judged expensive. The short book does two jobs: it is a source of alpha in its own right, and it hedges market exposure so returns depend more on selection than on market direction.

Exposure arithmetic (recap from Lesson 1):

- Gross exposure = long + short (measures leverage and risk)
- Net exposure = long − short (measures market directionality)
- A book with 130% long and 30% short is "130/30": gross 160%, net 100%, still fully market-exposed.
- Market-neutral targets net ≈ 0 and often beta ≈ 0; returns are then almost pure selection.

Variants seen in industry:

| Variant | Net exposure | Return driver | Typical desk |
|---|---|---|---|
| Fundamental long/short | ~40–70% long-biased | Stock selection + market beta | Discretionary equity HF |
| Equity market-neutral | ~0% | Selection alpha only | Quant / stat-arb |
| 130/30 (extension) | ~100% | Long-only + short overlay | Institutional "liquid alt" |
| Quant value | ~0% or long-biased | Value factor premium | Systematic |

Two distinct return sources, worth separating early:

- Selection alpha: idiosyncratic, from being right on individual names. Hard, capacity-limited, decays with crowding.
- Value premium: a compensated factor (cheap stocks outperform expensive ones on average, Fama–French HML). Replicable and cheap to access, so it does not justify a performance fee on its own. This is the Ilmanen/Ang point from Lesson 1: separate the harvestable beta from the skill.

---

## 2. The equity universe

Instruments the strategy trades and hedges with.

- Common stock: residual claim, voting. Watch multiple share classes (e.g. founder super-voting shares) — they change control and index eligibility.
- Indices: the benchmark and the beta hedge. Construction matters:
  - Cap-weighted (S&P 500, MSCI) — dominated by largest names; most institutional benchmarks.
  - Price-weighted (Dow, Nikkei 225) — an accident of history, distorted by share price level, not used for serious allocation.
  - Equal-weighted — implicit small-cap and rebalancing tilt.
- ETFs and index futures: used to strip out market beta cheaply (short the index, keep the single-name longs) and to express sector views.
- Single-stock shorting mechanics (practical, often skipped in textbooks):
  - You borrow the share, sell it, post collateral, pay a borrow fee.
  - "Hard-to-borrow" names carry high fees that can exceed the expected alpha.
  - Short squeezes and recalls force covering at the worst time; borrow can disappear.
  - Dividends on the shorted stock are paid by the short seller.
  This is why crowded shorts are dangerous even when the thesis is correct.

---

## 3. Fundamental analysis: the inputs

Abbreviations used below: **EBIT** (earnings before interest and taxes, i.e. operating profit); **EBITDA** (earnings before interest, taxes, depreciation and amortization, a rough proxy for operating cash flow); **GAAP** (Generally Accepted Accounting Principles, the standardized US accounting rules; "non-GAAP" or "adjusted" numbers depart from them).

Valuation runs on three financial statements. Know what each drives.

| Statement | Key outputs used in valuation |
|---|---|
| Income statement | Revenue, margins, EBIT, EBITDA, net income, EPS |
| Balance sheet | Debt, cash, equity, invested capital, book value |
| Cash flow statement | Operating cash flow, capex, free cash flow |

Practical filters before trusting any multiple:

- Earnings quality / accruals: net income can be inflated by accounting choices while cash flow lags. Large positive accruals (net income >> operating cash flow) predict weaker future returns (Sloan accrual anomaly).
- GAAP vs. adjusted / non-GAAP: management-defined "adjusted EPS" strips out costs they call one-off. The gap between GAAP and adjusted numbers is a red flag when persistent.
- Share count: use diluted shares (includes options, convertibles, RSUs). Buybacks raise EPS without raising earnings — decompose EPS growth into earnings growth vs. share shrinkage.

---

## 4. Relative valuation (multiples)

Value a company by comparison to peers. Fast, market-anchored, and the industry default for a first screen.

Core multiples:

| Multiple | Formula | Use / caveat |
|---|---|---|
| P/E | Price / EPS | Most common; meaningless if EPS ≤ 0; distorted by leverage and accounting |
| Forward P/E | Price / expected EPS | Forward-looking but depends on analyst estimates |
| PEG | (P/E) / earnings growth % | Crude growth adjustment; sensitive to the growth input |
| EV/EBITDA | Enterprise value / EBITDA | Capital-structure neutral; preferred when leverage differs across peers |
| EV/Sales | EV / revenue | For loss-making or early-stage firms with no earnings |
| P/B | Price / book value | Banks, insurers, asset-heavy; weak where intangibles dominate |
| FCF yield | Free cash flow / market cap | Harder to manipulate than earnings |
| Dividend yield | DPS / price | Income and quality proxy |

Enterprise value: `EV = market cap + total debt − cash + minority interest + preferred`. You can use EV multiples when comparing firms with different leverage; equity multiples (P/E, P/B) mix in the financing decision.

Comparables method in practice:

1. Define a peer set (same sector, size, growth, margin profile). Peer selection is where most of the subjectivity and the error lives.
2. Compute the multiple for each peer, take median (not mean; robust to outliers).
3. Apply to the target's fundamental to get an implied value.
4. The gap between implied and market price is the raw signal.

Weakness: relative valuation tells you a stock is cheap *relative to peers*, not cheap in absolute terms. If the whole sector is overpriced, the "cheapest" name still falls.

---

## 5. Intrinsic valuation (DCF) — Damodaran

**Cash flow** is the actual money moving in and out of a company over a period; **free cash flow (FCF)** is what remains after operating costs and investment, and a **discounted cash flow (DCF)** values the company by adding up its expected future free cash flows, each discounted to what it is worth today.

DCF (discounted cash flow) says a company is worth the money it will generate in the future, converted into today's value. Money received later is worth less than money today, so each future year's cash is reduced ("discounted") before adding it up. This gives a value that does not depend on the current share price.
 
The calculation has four steps:
 
1. Estimate free cash flow to the firm (FCFF) for each of the next few years (usually 5–10). This is operating profit after tax, plus depreciation (a cost on paper that uses no cash), minus money spent on new equipment (capex) and on growing day-to-day operations (working capital).
2. Discount each year's cash flow using WACC (weighted average cost of capital), the average return that lenders and shareholders require. The higher the WACC, the less future cash is worth today.
3. Add a terminal value for all the years after the forecast. It assumes cash flows keep growing forever at a small, steady rate (g), usually close to long-run economic growth.
4. The sum of steps 2 and 3 is the value of the whole business (enterprise value). Subtract net debt to get the value that belongs to shareholders, then divide by the number of shares to get a value per share.

You can also value only the shares. In that case, start from the cash left for shareholders after the company has paid its lenders (free cash flow to equity, FCFE). Discount it at the return that shareholders require (cost of equity), not WACC. The result is the shareholders' value directly, so there is no need to subtract debt.

The three drivers that dominate the output: (i) growth rate g, (ii) discount rate WACC / cost of equity, (iii) terminal value, which usually accounts for the majority of the total. Small changes in g or WACC move value sharply — always run a sensitivity table over both.

Reverse DCF (the practically useful version): instead of forecasting growth to get a price, take the current market price as given and solve for the growth rate the market is implying. Then judge whether that implied growth is plausible. This sidesteps the false precision of a forward DCF and is how experienced discretionary managers actually use it.

---

## 6. From valuation to a trade signal

This is the step the textbooks underweight and the desk cares about most.

Discretionary path: analyst builds a valuation, identifies a catalyst (earnings, spin-off, management change, margin inflection), sizes the position by conviction and liquidity, sets a target and a stop. Without a catalyst, a cheap stock can stay cheap indefinitely (the value trap).

Systematic path: turn valuation into a ranked factor across the whole universe.

1. Pick a value metric: earnings yield (E/P), book-to-market, EV/EBITDA inverse, FCF yield.
2. Rank the universe each period; go long the cheapest decile, short the most expensive.
3. Neutralise unwanted exposures: sector-neutral (rank within sector), beta-hedge with index futures, cap size and liquidity.
4. Backtest with point-in-time data; measure the spread return (top minus bottom).

Value factor context (Ilmanen, Ang): the long-short value spread is the HML factor. It is real over the long run but has long, deep drawdowns — notably the 2007–2020 underperformance of value vs. growth, which drove the debate over whether the premium is dead or just mismeasured (book value ignores intangibles like R&D and brand, which distorts P/B for modern firms). This is the central practical caveat for any value strategy today.

## 7. Multifactor models and their practical applications
 
A factor model explains a stock's or fund's return as the sum of its exposures to a few common, rewarded risk sources, plus a leftover. That leftover is the real measure of skill. Each factor is itself a long/short portfolio (long the favoured group, short the other).
 
- CAPM: only market exposure is rewarded. Expected excess return = beta × market excess return; everything else is "alpha". It leaves too much unexplained: cheap and small stocks earned more than their beta justified.
- Fama-French 3-factor (1993): adds size (SMB, small minus big) and value (HML, high minus low book-to-market, the value premium from section 6).
- Carhart 4-factor (1997): adds momentum (WML - winners minus losers, past winners keep rising).
- Fama-French 5-factor (2015): adds profitability (RMW - robust minus weak, profitable firms outperform) and investment (CMA - conservative minus aggressive, cautious investors outperform aggressive ones).
Each model is a longer checklist of known reasons a stock earns a return; your return is a weighted mix of these factors plus residual alpha.
- Mention EMH.
 
Practical applications
 
- Separating skill from luck of style. Compare a fund's returns with the returns of the factors (using a regression). Part of the fund's return can be explained by its exposure to the factors, for example because it simply holds cheap stocks. That part requires no special skill, since anyone can get the same exposure through a low-cost factor ETF. Only the return left unexplained (alpha) shows the manager's own skill, and only that part is worth paying hedge fund fees for.
- Understanding where returns and risks come from. The same regression shows why a fund made or lost money, for example because it was betting on value or on small companies. It also shows hidden risks. Two funds may hold completely different stocks but both be heavily exposed to value, so they will lose money at the same time when value stocks do badly, as happened in 2018–2020.
- Building portfolios. Diversification works best across factors, not across individual stocks. Holding many stocks with the same factor exposure gives little protection. Combining factors that do not move together, such as value and momentum (which tend to move in opposite directions), gives smoother returns.
Limits: premia are long-run averages with deep drawdowns, published "factors" number in the hundreds (most fail out-of-sample, the "factor zoo"), and exposures are estimated with error.

---

## 7. Practical problems

- Value trap: cheap because the business is deteriorating, not because it is mispriced. Requires a quality screen (profitability, balance-sheet strength) on top of cheapness.
- Accounting distortion: multiples built on manipulated or non-comparable earnings. Cross-check with cash flow.
- Buyback-driven EPS: rising EPS from shrinking share count, not growth.
- Look-ahead and survivorship bias: using restated financials or a universe that excludes delisted firms inflates any backtest. Use point-in-time fundamentals.
- Crowding: popular value signals are arbitraged down and can unwind violently (the quant equity deleveraging of August 2007).
- Short-side asymmetry: a short that goes against you grows in the book; losses are unbounded and borrow can be recalled.

---

## Additional reading (for this lesson)

- Bodie, Kane & Marcus, *Essentials of Investments* — Ch. 13 (Equity Valuation: multiples, dividend discount and free cash flow models) and Ch. 14 (Financial Statement Analysis: ratios, earnings quality) for sections 3–5; Ch. 7 (CAPM and APT, including multifactor models and Fama-French) for section 7. A simpler introduction than Damodaran, suitable for students without a finance background.

Link to selected chapters will be shared via email.

- Insights by Asset Management firms (The BEAT by Morgan Stanley, Ivestment Strategy Focus by BNP Paribas, Investement commentaries and Market outlooks by Blackrock) - these insights can be slightly delayed but are great source of information on cross-asset investmens and markets.

## 8. Reading assignment (for next lessons topic)

Papers:

- Estrella & Trubin (2006), *The Yield Curve as a Leading Indicator: Some Practical Issues.* [Link](https://www.newyorkfed.org/medialibrary/media/research/current_issues/ci12-5.pdf)

- Koijen, Moskowitz, Pedersen & Vrugt (2018), *Carry.* [Link](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2298565)

- Cochrane & Piazzesi (2005), *Bond Risk Premia.* [Link](https://static1.squarespace.com/static/5e6033a4ea02d801f37e15bb/t/5edad244c49cf328503ac1bc/1591398982159/cochrane_piazzesi_bond_risk_premia.pdf)

Core (course list):
 
- Ilmanen, *Expected Returns* — the bond-risk-premium, rates, and carry chapters.
- Lhabitant, *Handbook of Hedge Funds* — the global macro and managed-futures (CTA) chapters.

Supplementary:
 
- Tuckman & Serrat, *Fixed Income Securities: Tools for Today's Markets* — the standard practitioner text on duration, convexity, curve construction, and DV01-hedged trades. 
- Adrian, Crump & Moench (NY Fed, 2013), "Pricing the Term Structure with Linear Regressions" — the term-premium model.
- Ang, *Asset Management* — the factor view of fixed income (duration and carry as factors), for the systematic-macro framing.

---

## 9. Home tasks 

All use free data (e.g. `yfinance`, stooq, Damodaran's online datasets, Ken French data library). Deliver short report + reproducible code via email (one email per team)

1. Comparables valuation and call.
   Pick one company and 5–7 peers. Compute P/E, EV/EBITDA, P/B, FCF yield for each. Value the target off peer medians, state whether it is cheap or expensive and by how much, and decide long or short with a one-paragraph thesis. Discuss where peer selection changed the answer.

2. DCF and reverse DCF on a single stock.
   Build a 5-year FCFF DCF with an explicit terminal value. Report the intrinsic value and its sensitivity to g and WACC (2-D table). Then run the reverse DCF: solve for the growth rate implied by the current price and judge whether it is realistic versus analyst consensus.

3. Value factor backtest.
   Take an index universe (e.g. S&P 500 constituents or a French-library value sort). Rank by earnings yield or book-to-market each month, form a long-top-decile / short-bottom-decile portfolio, and backtest 10+ years. Report annualised return, volatility, Sharpe, max drawdown, and plot the value drawdown of 2018–2020. Comment on whether the premium survived in your sample.

4. Earnings-quality signal.
   For a set of 20–30 firms, collect GAAP EPS and company-reported adjusted EPS. Quantify the gap and the accrual ratio (net income − operating cash flow). Test whether large adjustments or high accruals are associated with weaker subsequent returns. Short write-up of what you find.

5. Market-neutral book construction.
   Select 5 longs (cheapest) and 5 shorts (most expensive) within one sector. Beta-hedge to net ≈ 0 using an index ETF or future. Backtest over 2–3 years, report gross vs. net exposure through time, realised beta, and performance versus a naive long-only value screen. Discuss borrow cost as a drag on the short book.
