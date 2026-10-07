# Lesson 1 — Introduction; Hedge Fund Strategy Taxonomy and Asset Class Mapping

---

## 1. What defines a hedge fund

A hedge fund is defined by its **legal and structural form, not by its strategy**. The label describes the vehicle; the return-generating behaviour is the strategy. 

Structural features (contrast with a long-only mutual/UCITS fund):

| Feature | Hedge fund | Long-only mutual fund |
|---|---|---|
| Investor base | Accredited / professional / qualified only | Retail |
| Offering | Private placement | Public |
| Regulation | Light (registration, disclosure) | Heavy (holdings, leverage caps) |
| Short selling | Permitted | Usually prohibited |
| Leverage | Permitted, often high | Capped / minimal |
| Derivatives | Central to many strategies | Restricted |
| Fees | Management + performance (historically "2 and 20"; now typ. ~1–1.5% + 15–20%, high-water mark, sometimes hurdle) | Management only |
| Liquidity terms | Lock-ups, notice periods, gates | Daily |
| Mandate | Absolute return | Relative to a benchmark |

The economically important consequences: the ability to **short**, use **leverage**, use **derivatives**, and hold **illiquid** positions is what expands the strategy set beyond long-only. Performance fees create the incentive structure and the survivorship/selection issues.

---

## 2. Why classify by strategy

- The "hedge fund" wrapper says nothing about risk or return drivers; the strategy does.
- Strategy families have low mutual correlation, so classification is a prerequisite for benchmarking, allocation, and risk budgeting.
- Index providers must assign a fund to a bucket to build investable style indices; this is where the industry taxonomy comes from.

A caution: self-reported style, survivorship bias, and backfill bias contaminate hedge-fund index data. Treat any "average returns by strategy" number as an upper-biased estimate.

---

## 3. Three types of classification

No single axis is sufficient. Use all three.

### 3.1 Industry / index-provider taxonomy (the practical standard)

Providers: HFR (HFRI), Credit Suisse Hedge Fund Index, BarclayHedge, Eurekahedge, Morningstar. Conventions differ in detail but converge on four to five top-level families. HFR's top level:

| Family | Core idea | Representative sub-strategies |
|---|---|---|
| Equity Hedge (Long/Short Equity) | Long undervalued / short overvalued equities | Fundamental value, fundamental growth, equity market neutral, quantitative directional, short bias, sector |
| Event-Driven | Profit from corporate events | Merger (risk) arbitrage, distressed/restructuring, special situations, activist, credit arbitrage |
| Macro | Directional bets on macro variables | Discretionary thematic, systematic (CTA / managed futures), currency, commodity |
| Relative Value | Arbitrage price relationships, low net direction | Convertible arbitrage, fixed-income arbitrage, volatility arbitrage, asset-backed |
| Multi-Strategy / Fund of Funds | Combine several of the above | — |

### 3.2 Analytical axes (how to reason about any strategy)

Classify each strategy along these dimensions rather than memorising labels:

- **Directionality**: directional (net long/short market exposure) vs. relative-value / market-neutral (hedged net exposure, bets on spreads).
- **Decision process**: discretionary (manager judgment) vs. systematic (rules/model-driven).
- **Asset-class focus**: single-class specialist vs. cross-asset.
- **Return source**: mispricing/arbitrage (convergence) vs. risk premium (compensation for bearing risk) vs. genuine alpha (skill).
- **Horizon and liquidity**: intraday to multi-year; liquid listed instruments to distressed/private.
- **Capacity**: how much capital the strategy absorbs before returns decay.

### 3.3 Factor / risk-premia lens (the academic spine — Ilmanen, Ang)

The key idea, developed across the course: much of what is sold as hedge-fund "alpha" is **alternative beta** — compensation for systematic risk factors that can be harvested cheaply and systematically.

- Ilmanen (*Expected Returns*): decompose any return into asset-class premia + style/strategy premia + alpha. Recurring style premia: **carry, value, momentum, defensive/low-volatility, liquidity**. These reappear across asset classes.
- Ang (*Asset Management*): "factors are to assets what nutrients are to food." Assets are bundles of factor exposures; factors earn premia because they pay off poorly in **bad times**. Factor investing = targeting these exposures directly.

---

## 4. Strategy families 

Short reference, we will talk more about these strategies in detail in next lessons.

**Long/short equity** — Long positions in expected outperformers, short in expected underperformers. Net exposure ranges from directional (net long) to **equity market neutral** (~zero net, zero beta). Return source: stock selection (alpha) + residual market/factor beta. Asset class: equities, indices, sectors. Valuation-driven variant uses fundamentals (Lesson 2). Key risk: factor crowding, short squeezes.

**Event-driven** — Returns tied to corporate events, largely uncorrelated with market direction.
- *Merger (risk) arbitrage*: long target, short acquirer; capture the deal spread; risk is deal break.
- *Distressed*: securities of firms in or near bankruptcy; requires legal/restructuring analysis.
- *Special situations / activist*: spin-offs, buybacks, forcing corporate change.
Asset classes: equities and credit. Return source: event risk premium + analysis. (Lesson 7 covers distressed/event.)

**Relative value** — Bet on price relationships between related instruments; low net direction.
- *Convertible arbitrage*: long convertible bond, short the underlying equity; extract mispriced optionality.
- *Fixed-income arbitrage*: exploit pricing anomalies across bonds/curve/swap spreads; typically levered, tail-risk-prone (cf. LTCM).
- *Volatility arbitrage*: trade implied vs. realised volatility via options.
Return source: convergence + risk premium. Asset classes: credit, rates, convertibles, volatility.

**Global macro** — Directional positions on macro variables (rates, currencies, commodities, equity indices) driven by views on business cycles, policy, and inflation.
- *Discretionary*: manager thesis (Lesson 3).
- *Systematic / CTA (managed futures)*: rule-based trend-following and carry across futures markets. Often long volatility / crisis-convex.
Asset classes: all liquid futures and FX. Return source: macro risk premia + timing.

**Emerging markets** — Long-biased exposure to EM equity, debt, FX; illiquidity and regime risk. (Interacts with Lesson 4: US/DM–EM transmission, correlation, regime change.)

**Multi-strategy / fund of funds** — Allocate across the above for diversification. FoF adds a second fee layer.

**Systematic/quant and crypto** — Statistical arbitrage, momentum/mean-reversion (Lesson 6), technical signals (Lesson 5), NLP/sentiment (Lesson 7), algorithmic execution (Lesson 10), and digital-asset strategies (Lesson 9). Return source: statistical edges; principal risk is **backtest overfitting** (López de Prado).

---

## 4A. Multi-manager platform firms (the "pod" model)

The largest and most influential firms — **Citadel, Millennium Management, Point72, Balyasny, ExodusPoint, Schonfeld** — are not single-strategy funds but **multi-manager platforms**. They are an organisational structure and a platform runs many of the strategies above simultaneously.

Mechanics:

- **Pods.** Capital is split across dozens to hundreds of semi-autonomous teams ("pods"), each running its own book (often long/short equity, but also macro, fixed income, quant). Pods compete for capital.
- **Tight risk limits and stop-outs.** Each pod has a strict drawdown limit (e.g. ~5–10%); breaching it cuts the pod's capital or shuts it down. This enforces low volatility at the firm level.
- **Aggregate market neutrality.** Individual pods take directional bets, but the platform nets exposures toward market-neutral, targeting return from dispersion across pods rather than market beta.
- **Centralised risk, financing, and execution.** Leverage, prime-broker relationships, treasury, and risk oversight are pooled centrally; pods receive risk budget, not free rein. Firm-wide leverage is high (often 4–10× gross).
- **Pass-through fees.** Instead of a fixed management fee, platforms pass operating costs (data, compensation, technology) directly to investors on top of the performance fee — a costlier arrangement justified by consistent, low-drawdown returns.

The pod model industrialises the strategies studied in upcomming lessons — the same signals, wrapped in aggressive risk control and diversification across many independent books. It also concentrates talent and crowds trades, a systemic-risk concern regulators now track.

---

## 5. The main asset classes

Strategies are applied *to* asset classes. Establish the map of what exists and how each is valued and behaves.

| Asset class | Instruments | Valuation basis | Primary macro sensitivity |
|---|---|---|---|
| Equities | Stocks, indices, sectors | Discounted cash flows / multiples (P/E, EPS) | Growth |
| Sovereign fixed income / rates | Govt bonds, swaps, futures | Term structure, discounting | Rates, inflation |
| Credit | Corporate bonds, loans, CDS | Spread over risk-free, default risk | Growth, credit cycle |
| Convertibles | Convertible bonds | Bond floor + embedded equity option | Equity + rates + vol |
| Currencies (FX) | Spot, forwards, futures | Rate differentials, PPP, flows | Rates, terms of trade |
| Commodities | Energy, metals, agriculture (mostly futures) | Spot + cost of carry / convenience yield | Inflation, real economy |
| Volatility | Options, VIX products, variance swaps | Implied vs. realised vol | Risk regime |
| Real estate / REITs | Listed and private property | Income yield + appreciation | Rates, inflation, growth |
| Private equity | Buyout, growth equity | Illiquid, mark-to-model | Growth, credit, liquidity |
| Venture capital | Early-stage equity | Option-like, highly illiquid | Idiosyncratic, liquidity |
| Natural resources / infrastructure | Land, energy assets | Cash-flow + real-asset | Inflation |
| Crypto | Coins, stablecoins, staking | No consensus; network/flow models | Idiosyncratic, liquidity, risk regime |

Organising dimensions to note: **liquidity**, **cash-flow basis** (income vs. capital gain), **valuation method** (market price vs. mark-to-model), and **factor exposure** (growth vs. inflation, carry, momentum).

---

## 6. Cross concepts to remember 

- **Long / short, gross vs. net exposure, leverage.** Gross = long + short; net = long − short. Market-neutral targets net ≈ 0.
- **Alpha vs. beta.** Beta = exposure to a priced systematic factor (compensated, replicable). Alpha = return not explained by known factors (skill, or unmeasured beta).
- **Hedge.** Offsetting a specific risk while retaining a targeted exposure — the origin of the term.
- **Arbitrage: true vs. statistical.** True arbitrage = riskless, self-financing, positive payoff (rare). "Arbitrage" in HF usage is usually *statistical* — expected convergence with residual risk.
- **Risk premium vs. mispricing.** A premium is compensation for risk and persists; a mispricing is an error that closes. They require different justification and have different capacity/decay.
- **Absolute vs. relative return.** Hedge funds target absolute return; the benchmark is often cash or zero, not an index.

---

## 7. Reading assignment for Lesson 1

- Lhabitant, *Handbook of Hedge Funds* — introductory chapters on the industry and the strategy classification.
- Ilmanen, *Expected Returns* — introduction and the return-decomposition / risk-premia framework.
- Ang, *Asset Management* — introductory chapters on factors ("factors, not assets").

## 7.1 Reading for Lesson 2 

Core (course list, mandatory for next week):

- Damodaran, *Investment Valuation* — relative valuation (multiples) and intrinsic (DCF) chapters; the reverse-DCF idea.
- Lhabitant, *Handbook of Hedge Funds* — long/short equity chapter (mechanics, exposure, shorting).
- Ilmanen, *Expected Returns* — the value premium chapter.
- Ang, *Asset Management* — value factor ("factors, not assets").

Supplementary (stronger on specific):

- Gray & Carlisle, *Quantitative Value*
- Koller, Goedhart & Wessels (McKinsey), *Valuation: Measuring and Managing the Value of Companies* 
- Schilit, Perler & Engelhart, *Financial Shenanigans* 
- Graham & Dodd, *Security Analysis* 

---

## 8. Warm-up exercises

Short written answers (a few sentences each), emailed to me by the end of the week and then discussed verbally during next class. 

1. In your own words, name two features that make a fund a "hedge fund" and explain why the term describes the structure, not the strategy. Give one example of two hedge funds that would behave very differently.
2. Define alpha and beta. A fund returns 12% in a year when the market returns 10% and the fund has a market beta of 1. How much of the 12% is beta and how much is alpha? What would the answer be if beta were 0.5?
3. Pick any one strategy family from Section 4 (e.g. long/short equity, merger arbitrage, global macro). State which asset class it mainly trades and name the single biggest risk it faces.
4. Choose your team members for the next course assignments.