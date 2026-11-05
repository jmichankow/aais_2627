# Lesson 5 — Technical and Chart-Based Strategies

In this lesson we will cover parts of technical analysis used by systematic (rule-based) funds: as a set of mathematical transformations that convert a price series into a trading signal. Each indicator is a formula applied to past prices and volume. The lecture has two parts. The first covers the mechanics of the **main indicators** — moving averages, RSI, and Bollinger Bands — and how each is converted into an entry and exit rule. The second covers how to test such a rule, since this is the area of the course where a **backtest** most easily overstates a strategy's value.

Technical analysis has a weak academic reputation, and most of it is **not supported by evidence**. One part of it — trend and momentum — does survive statistical testing, and the funds that trade it systematically manage large amounts of capital. The appropriate approach is therefore measurement rather than acceptance or rejection: specify the rule, then test whether its historical performance reflects a real effect or an artifact of the testing procedure.

Primary reading: Chan, *Algorithmic Trading: Winning Strategies and Their Rationale* (signal construction and backtesting); López de Prado, *Advances in Financial Machine Learning* (backtest overfitting, used in Section 6).

---

## 1. Technical signals: definitions

A technical strategy maps the history of prices (and sometimes volume) up to the current day to a position for the next day. The position is usually expressed as a number such as +1 (long), 0 (flat), or −1 (short), and can take intermediate values.

    position_t = f(**price history up to time t**)

An indicator is an intermediate calculation, such as a moving average or an oscillator value. A signal is the rule that converts an indicator into a position. The distinction matters because the same indicator can produce opposite positions depending on the rule (see Bollinger Bands, Section 4).

At least two requirements define a valid backtest of such a rule:

**Only past information may enter the position**. The position for day *t* may use prices up to the close of day *t−1*, or the close of *t* if trades are assumed to execute at that close; the assumption must be stated. If the position for day *t* uses prices from *t* or later, the backtest contains look-ahead bias and its results are invalid. This error is easy to introduce accidentally in code.

**Trading costs must be included**. Each change in position is a trade that pays a spread and commission. A rule that changes position frequently can have positive gross returns and negative net returns. For technical strategies, which often trade frequently, costs are frequently the deciding factor rather than a minor adjustment.

Both requirements are revisited in Section 6.

---

## 2. Moving averages

A moving average smooths a price series so that its direction is easier to identify. It is the basis of trend-following, the technical approach with the strongest empirical support. However you still need to **treat it with a grain of salt**.

The simple moving average (SMA) over *N* days is the mean of the last *N* closing prices:

    SMA_N(t) = (P_t + P_{t-1} + ... + P_{t-N+1}) / N

A short window (e.g. 20 days) follows the price closely; a long window (e.g. 200 days) responds slowly and captures only the major trend. The SMA weights a price from *N* days ago equally with yesterday's price, and reacts with a lag.

The exponential moving average (EMA) reduces this lag by weighting recent prices more heavily:

    EMA(t) = α · P_t + (1 − α) · EMA(t−1),   with α = 2 / (N + 1)

For N = 20, α ≈ 0.095: the current price receives about 9.5% weight, and earlier prices share the remainder with geometrically decaying weights. The EMA responds faster than the SMA of the same length but is noisier.

The standard rule built from moving averages is the crossover, using a fast and a slow moving average:

- long when the fast MA is above the slow MA,
- flat or short when the fast MA is below the slow MA.

The "golden cross" (50-day above 200-day) and "death cross" (the reverse) are named cases of this rule. A crossover is a trend-following rule: it performs well in sustained trends and poorly in sideways markets, where the averages cross repeatedly (whipsaw) and each crossing incurs a transaction cost.

MACD (Moving Average Convergence Divergence) restates the same idea. It is the difference between a fast and a slow EMA, with a third EMA of that difference as a signal line:

    MACD = EMA_12 − EMA_26
    signal line = EMA_9(MACD)

A long signal occurs when MACD crosses above its signal line. MACD contains no information beyond a two-EMA crossover.

---

## 3. RSI and oscillators

Oscillators assume that **prices move too far too quickly** and then revert, which is the opposite assumption to moving averages. An oscillator is bounded (typically 0–100) and is used to identify overbought and oversold conditions.

The **Relative Strength Index (RSI)** is the standard oscillator. Over a window *N* (conventionally 14 days):

    RS = (average gain over N days) / (average loss over N days)
    RSI = 100 − 100 / (1 + RS)

Example: if average up-moves over the last 14 days are twice the size of average down-moves, RS = 2 and RSI = 100 − 100/3 ≈ 66.7. If gains and losses are equal, RS = 1 and RSI = 50. RSI approaches 100 when recent moves are almost all up, and 0 when they are almost all down.

The standard rule: RSI above 70 is overbought (sell), below 30 is oversold (buy). This is a mean-reversion rule, since it takes a position against the recent move. It is the opposite of a moving-average crossover, which takes a position in the direction of the recent move. Which rule performs better depends on whether the asset trends or mean-reverts. Individual equities and pair spreads tend to mean-revert at short horizons; equity indices and commodities tend to trend at longer horizons. Selecting the indicator to match the asset's behaviour is the main design decision.

---

## 4. Bollinger Bands

Bollinger Bands place a volatility envelope around a moving average. The middle band is a simple **moving average** (usually 20-day); the upper and lower bands are set a fixed **number of standard deviations** above and below it (usually two):

    middle = SMA_20(P)
    upper  = middle + k · σ_20(P)      (k = 2)
    lower  = middle − k · σ_20(P)

where σ_20 is the standard deviation of the last 20 prices. Because the bands use a rolling standard deviation, they widen when volatility rises and narrow when it falls. This is their main property: the threshold adapts to the current volatility level rather than being fixed.

Bollinger Bands support two opposite rules:

- **Mean-reversion**: price touching the upper band is treated as stretched (sell); touching the lower band (buy). This suits range-bound markets.
- **Breakout**: price closing outside a band is treated as a genuine move (buy an upper break, sell a lower break). This suits trending markets and is the opposite position.

The bands alone do not indicate which regime holds, so a naive band rule is unreliable, and practitioners combine bands with a separate trend filters (Section 5). Two derived quantities are used: %b, which locates price within the bands (0 at the lower band, 1 at the upper), and bandwidth, (upper − lower)/middle, which measures band width and is used to detect volatility contractions.

---

## 5. Constructing a trading rule

An indicator value is not a strategy. Converting it into one requires four decisions.

**Entry and exit**. "Buy when RSI < 30" requires a matching exit rule — for example, sell when RSI > 50, or after a fixed holding period. The exit affects performance as much as the entry.

**Position size**. Fixed size, or scaled by signal strength or volatility. Volatility targeting sizes the position inversely to recent volatility so that each trade contributes approximately equal risk. This often improves risk-adjusted performance more than adjusting the entry rule.

**Combining indicators**. Single indicators are weak. A common structure uses a slow indicator to set direction and a fast one to time entry — for example, taking RSI-oversold long entries only when the 200-day moving average is rising, so that dip-buying is restricted to uptrends. The trend filter removes the mean-reversion trades that fail in downtrends.

**Trading frequency and cost**. After specifying the rule, count how often it trades and multiply by a realistic per-trade cost before evaluating returns.

These four decisions are not visible on a price chart and account for the difference between chart interpretation and a systematic strategy.

---

## 6. Backtesting

**This section is the reason technical analysis is included in a quantitative course. ** Any rule above can be made to appear profitable on historical data. The task is to determine whether that appearance reflects a real effect.

**Look-ahead bias**. The position at time *t* must not use information unavailable at *t*. A common form is normalising by a full-sample statistic (e.g. the whole period's mean or standard deviation) inside a signal, which introduces future information. Every parameter must be computed only from data available at the time.

**In-sample overfitting**. If many parameter combinations are tested and only the best is reported, that result is largely due to chance and typically fails out of sample. The minimum control is a train/test split: choose parameters on an earlier portion of the data, then evaluate once on a later portion. Walk-forward testing extends this by re-fitting on a rolling window and testing on the next period.

**Multiple testing**. This is the central issue in López de Prado's treatment. Testing many strategies produces some with a high Sharpe ratio by chance, including on data with no real structure. The expected maximum in-sample Sharpe rises with the number of configurations searched. Reporting that maximum as a finding is the most common error in technical-strategy research. The deflated Sharpe ratio adjusts for the number of trials; a further control is to have a prior reason a strategy should work before testing it.

**Transaction costs**. A frequently trading rule can have positive gross returns and negative net returns after spread, commission, and market impact. Costs must be included from the start of the backtest.

**Regime dependence**. A rule that worked over one period can fail when the market regime changes — for example, the stock–bond correlation and trend relationships that reversed across inflation regimes (Lesson 4). Performance over a single regime is not evidence that the rule generalises.

Summary: a technical strategy with a strong backtest and no future value is easy to produce. A result should be treated as unreliable until it survives out-of-sample testing, cost accounting, and an adjustment for the number of configurations tested.

---

## 7. Empirical evidence

Trend and momentum are somwehat supported. Time-series momentum — the tendency of assets that have risen over the past 3–12 months to continue rising over the near term — is one of the more robust empirical patterns in finance, documented across equities, bonds, commodities, and currencies over long samples. Moving-average and crossover rules are approximate implementations of it. This is the basis of the managed-futures industry and connects to Lesson 6 (momentum and mean-reversion) and Lesson 3 (trend-following as crisis-convex macro).

Short-horizon mean-reversion is supported conditionally. Individual stocks and some pairs revert over days to weeks, which is the statistical-arbitrage material of Lesson 6. RSI-type rules are a weak proxy for it. The effect is real but capacity-limited and sensitive to costs.

**Most other technical methods are not supported**. Named chart patterns (head-and-shoulders, flags), the more elaborate oscillators, and precise support/resistance levels show little predictive power once tested properly and adjusted for data mining.

The conclusion for the course: a small, statistically supported subset of technical analysis (trend and short-term reversal) is real and sometimes traded; the remainder has little evidence behind it. Distinguishing the two by testing is the skill the following lessons on momentum, mean-reversion, and performance measurement build on.

## Reading (next lesson)

Papers

Momentum. Daniel and Moskowitz, “Momentum Crashes,” Journal of Financial Economics, [Link](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2371227)

Guijarro-Ordóñez, Pelger and Zanotti, “Deep Learning Statistical Arbitrage, [Link](https://arxiv.org/abs/2106.04028)

Primary reading 

Chan, *Algorithmic Trading: Winning Strategies and Their Rationale* (momentum, mean-reversion, pairs trading, cointegration, and backtesting); López de Prado, *Advances in Financial Machine Learning* (backtest overfitting, used in Section 7).
 
Supplementary reading 

- Vidyamurthy, *Pairs Trading: Quantitative Methods and Analysis* — the chapters on the cointegration approach to pairs, as a reference for Section 5.
- Jegadeesh and Titman (1993), "Returns to Buying Winners and Selling Losers" (*Journal of Finance*) — the paper that established cross-sectional momentum; [Link](https://www.bauer.uh.edu/rsusmel/phd/jegadeesh-titman93.pdf)
- Moskowitz, Ooi and Pedersen (2012), "Time Series Momentum" (*Journal of Financial Economics*), same as in previous lesson
- Ilmanen, *Expected Returns* (a syllabus book) — the momentum chapter, for the risk-premium interpretation and the cross-asset evidence in Section 8.

---

## Home tasks

Choose one. All use daily price data from a free source (e.g. `yfinance`, or Stooq CSVs) for a liquid asset with a long history — an equity index ETF (SPY), a single large-cap stock, or a commodity ETF. Positions must use only past data, and returns must be reported net of a stated per-trade transaction cost (e.g. 5 basis points per side). Report an equity curve and summary statistics (annualised return, volatility, Sharpe ratio, maximum drawdown), compared against buy-and-hold.

1. Moving-average crossover.
   Core: implement a fast/slow SMA crossover (e.g. 50/200) on an index ETF over 15+ years. Long when fast > slow, flat otherwise. Enforce no look-ahead (the position on day *t* uses averages through *t−1*). Report net-of-cost performance versus buy-and-hold, and report the number of trades and total cost paid.
   Stretch: sweep the fast/slow window pair over a grid and plot the Sharpe ratio surface. Then split the sample in half, select the best pair on the first half only, and evaluate it once on the second half. Report how far the out-of-sample Sharpe falls below the in-sample best.

2. RSI mean-reversion.
   Core: implement an RSI(14) rule — buy when RSI < 30, exit when RSI > 50 — on a single large-cap stock. Report net performance versus buy-and-hold.
   Stretch: apply the same rule to a trending asset (a commodity or index) and to a mean-reverting one (a single stock or a pair spread), and show that the same rule succeeds on one and fails on the other. Explain the result using Section 3.

3. Bollinger Bands.
   Core: on one price series, implement both the mean-reversion rule (sell upper touch, buy lower touch) and the breakout rule (buy upper break, sell lower break) using 20-day, 2σ bands. Report both net performance curves.
   Stretch: add a trend filter (e.g. the sign of the 200-day MA slope) that selects between the two rules — mean-reversion when flat, breakout when trending — and test whether the switched rule outperforms either fixed rule.

4. Data-mining demonstration.
   Core: take one real price series. Generate a large number (e.g. 500) of arbitrary technical rules — random moving-average pairs, random RSI thresholds, random band widths. Record each rule's in-sample Sharpe ratio and report the best. Then evaluate that single best rule on a later, untouched portion of the data.
   Stretch: repeat the search on a synthetic random-walk series with no real structure, and show that a high-Sharpe rule still appears. Explain why the best backtest from a large search is not evidence, referencing López de Prado.

5. Combined, volatility-targeted strategy.
   Core: build a filter-plus-trigger rule — take long entries from an RSI-oversold or lower-Bollinger-touch signal only when the 200-day MA is rising — and size each position by inverse recent volatility rather than fixed size. Compare its risk-adjusted metrics to the same signal with fixed sizing.
   Stretch: apply the strategy to three asset classes (equity, commodity, currency) and report whether performance generalises or is concentrated in one asset. Relate the risk-adjusted comparison to the metrics formalised in Lesson 8.

6. Choose any strategy from crackingmarkets.com (you might need to set up an account but most of them are completely free). Try to reproduce it on any set of assets (stocks would be the closest) 
