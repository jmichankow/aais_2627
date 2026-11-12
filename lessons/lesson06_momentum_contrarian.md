# Lesson 6 — Momentum and Mean-Reversion Strategies

This lecture covers the two return patterns that most systematic strategies exploit: **momentum** (recent returns continue) and *mean-reversion* (recent moves reverse). Lesson 5 introduced these indirectly, through indicators. A moving-average crossover is one implementation of momentum, and an RSI or Bollinger rule is one implementation of mean-reversion. This lecture treats the two patterns as return premia in their own right, defines the standard ways to construct positions from them, and adds two elements that indicator rules do not cover: **cross-sectional construction** (ranking many assets against each other) and **statistical arbitrage** (trading the spread between related assets).

Momentum and short-horizon mean-reversion are the two technical effects that can survive statistical testing (established at the end of Lesson 5). They are not in conflict. **They operate at different horizons and on different constructions**, and a large part of the skill is knowing which one applies to a given asset and holding period. The second half of the lecture covers backtesting, because these strategies trade frequently and their historical performance is easy to overstate.

Primary reading: Chan, *Algorithmic Trading: Winning Strategies and Their Rationale* (momentum, mean-reversion, pairs trading, cointegration, and backtesting); López de Prado, *Advances in Financial Machine Learning* (backtest overfitting, used in Section 7).

---

## 1. Definitions

A strategy maps the return history up to the current day to a position for the next day, as in Lesson 5:

    position_t = f(return history up to time t)

The two patterns differ in the sign of that mapping.

**Momentum** takes a position in the direction of the recent return: if an asset has risen, hold it long; if it has fallen, hold it short or flat. The premise is that a trend continues over the near term.

**Mean-reversion** takes a position against the recent move: if an asset has risen relative to some reference level, sell it; if it has fallen, buy it. The premise is that the price returns to a level.

Whether an asset trends or reverts depends mainly on the horizon. **At daily to weekly horizons, individual equities and spreads tend to revert. At horizons of several months, most asset classes tend to trend.** The two effects also interact: the standard momentum construction deliberately skips the most recent month precisely because short-horizon reversal works against it there (Section 3).

A second distinction is how many assets the rule looks at.

Time-series (absolute) **momentum looks at each asset on its own**: the position depends only on that asset's own past return (Section 2).

**Cross-sectional (relative) momentum ranks a group of assets against each other **and holds the past winners against the past losers (Section 3). It is a relative bet and does not depend on the direction of the market as a whole.

Statistical arbitrage is a mean-reversion strategy applied to the **spread between related assets** rather than to a single price (Section 5). It rests on the distinction between true and statistical arbitrage from Lesson 1: the convergence is expected, not guaranteed, and carries residual risk.

---

## 2. Time-series momentum

Time-series momentum sets the position on each asset from the sign of that asset's own trailing return over a formation window of length L.

    r_L(t) = P_{t-1} / P_{t-1-L} − 1        (trailing return, using data through t−1)
    position_t = sign(r_L(t))               (+1 if positive, −1 if negative)

A common choice is a 12-month formation window with a monthly holding period, rebalanced at month end. The position is +1 (long) after a positive trailing year and −1 (short) or 0 after a negative one.

This is the same idea as the moving-average crossover from Lesson 5. The sign of a trailing return and the sign of (fast MA − slow MA) both measure trend direction, and they give similar positions. Time-series momentum is the direct, parameter-light version. It is the return source of the managed-futures industry (in Lesson 3, trend-following as crisis-convex macro), where the same rule is run across dozens of futures markets at once and the positions are averaged.

The main design choices are the lookback window (how far back the trailing return looks) and the holding period (how long the position is held before re-evaluation). Longer formation windows produce slower, lower-turnover strategies. These two parameters are the first place overfitting enters, and Section 7 returns to them.

---

## 3. Cross-sectional momentum

**Cross-sectional momentum ranks a group of assets by their recent return and holds the top group against the bottom group**. It does not ask whether an asset rose in absolute terms, only whether it rose more than its peers.

The standard construction, from Jegadeesh and Titman (1993):

1. Each month, compute every asset's return over the past 12 months, excluding the most recent month. This is the "12-1" or "12 minus 1" formation return. The most recent month is skipped because short-horizon reversal (Section 4) contaminates it: last month's biggest winners tend to give some of it back next month, which works against momentum.
2. Sort the assets into groups (for example deciles, or thirds for a small universe).
3. Go long the top group (past winners) and short the bottom group (past losers), in equal weight. Hold for one month, then repeat.

Because the long and short legs are of similar size, the portfolio has a **net market exposure near zero. Its return comes from the winner group outperforming the loser group, not from the market direction**. This makes it a relative-value strategy rather than a directional one.

Cross-sectional momentum has one behaviour that time-series momentum shares but shows less sharply: momentum crashes. In a sharp market rebound after a large decline, the past losers (which are typically high-beta, distressed assets) rebound hardest, so the short leg loses heavily at the same time as the long leg gains little. The strategy can lose a large amount quickly, as it did in 2009. The average return is high but **the left tail is heavy**, which matters for the risk-adjusted measurement.

---

## 4. Mean-reversion

Mean-reversion trades the **deviation of a price from a reference level**, betting that the deviation shrinks. For a single asset the reference is usually a moving average, and the deviation is measured in standard deviations (a z-score):

    z_t = (P_t − SMA_N(t)) / σ_N(t)

where SMA_N is the N-day moving average and σ_N is the N-day standard deviation of price. A high positive z means the price is stretched above its recent average; a high negative z means it is stretched below.

    enter long  when z_t < −k       (price is low, expect it to rise)
    enter short when z_t > +k       (price is high, expect it to fall)
    exit        when z_t returns near 0

This is the Bollinger-band rule from Lesson 5 written as a z-score, with k playing the role of the band width. Framing it as a z-score makes the assumption explicit: the strategy treats the price as fluctuating around a stable level, so that a large deviation is temporary.

**That assumption is the weakness. If the level itself is moving (the asset is trending), then a price above its average is not stretched but leading**, and selling it produces a stream of small losses. This is why a naive single-asset reversion rule is unreliable and why the effect is far stronger on spreads than on outright prices. A spread between two related assets has a more genuine tendency to return to a level than a single price does, which is the subject of the next section.

Short-horizon reversal works at daily to weekly holding periods and reverses sign as the horizon lengthens, where momentum takes over. It is real but has two practical limits: it trades very often, so transaction costs are decisive, and it holds little capital per trade, so its capacity is limited.

---

## 5. Statistical arbitrage and pairs trading

Statistical arbitrage applies mean-reversion to the spread between related assets. The classic case is a pair (popular pairs trading strategies, ie on similar stocks or cryptos).

Take two assets whose prices move together for an economic reason (two firms in the same industry, an index and its futures, a commodity and its producers). Form a spread:

    s_t = log P^A_t − β · log P^B_t

where β is a hedge ratio that scales B so that the two legs offset. If A and B genuinely move together, the spread s has no trend and fluctuates around a constant level. Trade it with the same z-score rule as Section 4:

    z_t = (s_t − mean(s)) / sd(s)
    long the spread  (buy A, sell β·B)  when z_t < −k
    short the spread (sell A, buy β·B)  when z_t > +k
    exit when z_t returns near 0

Because **the position is long one asset and short the other, it has little exposure to the market or the industry**. Its **return comes from the spread converging**, not from either asset rising. This is the statistical-arbitrage return source: an expected convergence with residual risk, not a riskless profit (Lesson 1).

The construction requires more than correlation. Two assets can be highly correlated day to day and still **drift apart permanently**, in which case the spread trends and never reverts, and the trade loses. The property needed is cointegration: a linear combination of the two prices that is stationary (fluctuates around a fixed level) even though each price on its own wanders without one.

The standard test is the Engle-Granger two-step procedure:

1. Regress log P^A on log P^B by OLS. The slope is the hedge ratio β and the residual is the spread s.
2. Test whether the spread s is stationary, using an augmented Dickey-Fuller (ADF) test. If the test rejects the presence of a unit root, the spread is stationary and the pair is cointegrated. If it does not reject, the two prices are not tied together and should not be traded as a pair.

**Correlation measures whether two assets move together on a given day. Cointegration measures whether they stay together over time. Statistical arbitrage requires the second**. The idea extends from pairs to baskets: a portfolio of many assets can be constructed to be stationary and traded the same way, which is how most stat-arb desks operate.

The central practical problem is that cointegration is not stable. A relationship estimated on past data can weaken or break, at which point the spread stops reverting and the position accumulates losses instead. Section 7 covers how backtesting must account for this.

---

## 6. Common structure

The strategies in this lecture share one form: measure how far an asset (or spread) has moved, then take a position based on that measurement and its expected continuation or reversal. They differ in three choices.

**Direction**. Momentum trades with the recent move; mean-reversion trades against it. The same measurement (a trailing return, a z-score) supports both, with opposite signs, so the choice of direction must be justified by the asset and horizon, not read off the data after the fact.

**Breadth**. Time-series rules act on each asset alone. Cross-sectional and pairs rules act on assets relative to each other, which removes market exposure and turns the strategy into a relative bet.

**Exit and sizing**. As in Lesson 5, the exit rule and the position size affect performance as much as the entry. Volatility targeting (sizing inversely to recent volatility) is standard here because these strategies are run across many assets and equal risk per position is the usual goal.

---

## 7. Backtesting momentum and mean-reversion

These strategies trade often and are ranked or fitted on historical data, so their backtests fail in specific ways. The general points from Lesson 5 (look-ahead bias, in-sample overfitting, multiple testing, transaction costs) all apply. The following are the ones that bite hardest here.

**Transaction costs and turnover**. Cross-sectional momentum re-ranks and re-trades every month; short-horizon reversal trades every few days. Turnover is high, so the gap between gross and net return is large. A reversion strategy that looks strong before costs is frequently unprofitable after them. Costs must be in the backtest from the start, and turnover should be reported alongside return.

**Survivorship bias in cross-sectional tests**. A momentum backtest that ranks "the stocks in the current index" over the past twenty years is **invalid, because the current index excludes the firms that went bankrupt or were delisted**. Those are exactly the past losers the strategy would have shorted. Using only surviving names removes the worst outcomes from the short leg and inflates the result. A valid cross-sectional backtest needs a point-in-time universe that includes assets which later disappeared.

**Overfitting the windows**. The formation and holding periods (Sections 2 and 3) and the entry threshold k (Sections 4 and 5) are free parameters. Searching over many combinations and reporting the best reproduces the **multiple-testing problem from Lesson 5: the maximum in-sample Sharpe rises with the number of combinations tried, and it typically falls out of sample**. Choose parameters on an earlier portion of the data and evaluate once on a later portion, and prefer parameters that have a prior reason (the 12-1 momentum window is used because it is documented across markets, not because it maximised the backtest).

**Unstable cointegration**. A pair that is cointegrated in-sample can decouple out of sample. Fitting β and testing stationarity once on the full history and then reporting the trade over the same history assumes a relationship that may not have held throughout. The relationship should be re-estimated on a rolling window (walk-forward), and the backtest should include the periods where the pair broke, not just the periods where it worked.

**Regime dependence and crash risk**. Momentum earns steady returns and then loses a large amount in a sharp rebound (Section 3). A backtest that happens to end before such an event overstates the strategy badly. Performance should be reported with the drawdown and tail statistics of Lesson 8, not the average return alone, and across at least one regime change (Lesson 4).

The summary from Lesson 5 holds unchanged: a strong backtest is easy to produce and should be treated as unreliable until it **survives out-of-sample testing, cost accounting, a point-in-time universe, and an adjustment for the number of configurations tested**.

---

## 8. Empirical evidence

Momentum is one of the most robust documented effects. Cross-sectional momentum earns a positive average return in equities, and the same time-series pattern appears in bonds, commodities, and currencies over samples exceeding a century. It is stronger than most other systematic effects, which is why it appears in every asset class and underlies the managed-futures industry. Its main defect is the crash risk in Section 3: the average return is high but the left tail is heavy.

Value and momentum combine well. The value premium (Lesson 2) and momentum are negatively correlated, so a portfolio holding both is steadier than either alone. This pairing is a standard result and a reason both factors appear together in multi-factor funds.

Short-horizon reversal is real but capacity-limited. The daily-to-weekly reversal effect in individual stocks and spreads exists and is traded, but it turns over quickly, so costs consume much of the gross return, and it holds little capital, so large funds cannot rely on it alone.

Statistical-arbitrage returns have decayed. Simple pairs and basket stat-arb produced high risk-adjusted returns into the early 2000s and much lower ones since, as more capital entered and execution improved. This is the clearest course example of a real edge shrinking through crowding: the effect was genuine, and competition reduced it rather than a testing error inventing it.

The conclusion for the course: momentum and short-horizon reversal are the two technical effects with genuine statistical support, they operate at different horizons, and their historical performance is realistic only after the costs, universe, and overfitting controls of Section 7. The risk-adjusted measurement that separates a good backtest from a good strategy is formalised in Lesson 8.

---

## Strategies used in practice

The constructions above appear in the industry under standard names. The list below is a reference for which strategies these ideas correspond to and where they are run.

Momentum:
- Trend-following / managed futures (CTAs): time-series momentum run across dozens of liquid futures and FX markets at once. The largest practical use of momentum by capital.
- Cross-sectional equity momentum (12-1): long past-winner stocks, short past losers, held market-neutral. A standard factor for quant long/short equity funds.
- Dual momentum: combine absolute momentum (in or out of the market) with relative momentum (hold the strongest asset). Common in systematic asset-allocation funds.
- Sector and industry momentum: rank sector or industry ETFs instead of single stocks; lower turnover and higher capacity than single-name momentum.
- Residual (idiosyncratic) momentum: rank on the return left after removing factor exposure, which lowers the crash risk of plain momentum.
- Volatility-scaled / risk-managed momentum: size positions inversely to recent volatility to smooth returns and reduce the crash tail (Section 3).
- Post-earnings-announcement drift (PEAD): hold stocks in the direction of an earnings surprise for weeks after the report. Overlaps with event-driven strategies (Lesson 7).
- Intraday / short-term momentum: continuation over minutes to hours, traded by high-frequency and short-horizon systematic desks.

Mean-reversion and statistical arbitrage:
- Pairs trading (cointegration): the spread between two related assets, traded on a z-score (Section 5).
- Market-neutral statistical arbitrage: reversion on the residuals of a factor or PCA model across a large basket of stocks. The standard institutional form of stat-arb.
- Short-term equity reversal: buy recent losers and sell recent winners over one day to one week. High turnover and very cost-sensitive.
- Index / ETF arbitrage: trade an ETF or index future against its underlying constituents when they diverge.
- Fixed-income relative value: on-the-run versus off-the-run bonds, and curve (butterfly) trades, betting a temporary yield-spread dislocation converges. Run by relative-value and macro funds.
- Futures calendar-spread reversion: the spread between two delivery months of one commodity, traded around its usual level.
- Cross-sectional reversion in FX and futures: fade the assets that have diverged most from a peer group over a short horizon.

---

## Home tasks

All use daily price data from a free source (for example `yfinance` or Stooq CSVs) for liquid assets with a long history. Positions must use only past data (the position on day *t* uses data through *t−1*), and returns must be reported net of a stated per-trade transaction cost (for example 5 basis points per side). Report an **equity curve** and summary statistics (annualised return, volatility, Sharpe ratio, maximum drawdown), compared against a benchmark, and report turnover (number of trades and total cost paid).

1. Time-series momentum on one asset.
   On an index ETF over 15+ years, take the sign of the trailing 12-month return, rebalanced monthly, long when positive and flat when negative. Report net performance versus buy-and-hold, and the turnover.
   Stretch: sweep the formation window over a grid (for example 3 to 12 months), then split the sample in half, select the best window on the first half only, and evaluate it once on the second half. Report how far the out-of-sample Sharpe falls below the in-sample best, referencing López de Prado.

2. Cross-sectional momentum.
   On a universe of 20 to 30 liquid names (sector ETFs, or large-cap stocks), each month rank by the 12-1 formation return, go long the top third and short the bottom third in equal weight, hold one month, repeat. Report the net performance of the long-short portfolio and its net market exposure.
   Stretch: examine the strategy through a sharp rebound (for example 2009 or spring 2020). Report the worst monthly loss and the maximum drawdown, and explain the result using Section 3 (the loser leg rebounding hardest).

3. Pairs trading with a cointegration test.
   Pick two related assets (two stocks in one sector, or a commodity ETF and a producer ETF). Estimate the hedge ratio β by OLS, test the spread for stationarity with an ADF test, and if it is stationary, trade the z-score with a ±2 entry and 0 exit rule. Report net performance and the market exposure of the position.
   Stretch: re-estimate β and the spread mean and standard deviation on a rolling window (walk-forward) rather than once on the full sample. Compare the walk-forward result to the fixed-parameter result, and identify any period where the pair decoupled (Section 7).

4. Short-horizon reversal.
   On a single large-cap stock, implement a z-score reversion rule (N-day moving average and standard deviation, long when z < −1, exit at 0). Report net performance versus buy-and-hold, and report how the result changes when the per-trade cost is doubled.
   Stretch: apply the same z-score, with opposite sign, as a momentum rule on the same asset, and vary the horizon N from a few days to several months. Show at which horizon the reversion rule stops working and the momentum rule starts, and relate this to Section 1.

5. Momentum versus mean-reversion on the same asset.
   On one asset, implement both a momentum rule (trailing-return sign) and a reversion rule (z-score) over the same sample, and report which one earns the higher net risk-adjusted return.
   Stretch: build a switch that selects between the two using a trend or volatility filter (for example, run momentum when the 200-day average is rising and reversion when it is flat), and test whether the switched rule beats both fixed rules out of sample. Report the comparison using the risk-adjusted metrics of Lesson 8.
