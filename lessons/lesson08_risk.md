# Lesson 8 — Risk-Adjusted Performance Metrics

This lecture covers how to judge a strategy's performance once its return series exists. Every previous lesson produced a rule that generates returns, and every one deferred the question of how to compare those returns to this lecture. **A raw return number is not enough**, because a strategy can earn a high return by taking large risk, and two strategies with the same average return can differ completely in how they lose money. The task is to **measure return relative to the risk taken**, and to do so with measures that are not easily fooled.

The lecture has three parts. The first defines the risk measures: **volatility and its shortcomings**, then the downside and tail measures that address those shortcomings. The second defines **drawdown and the risk-adjusted ratios** built from these measures, and asks which of them matter for which kind of strategy. The third covers **how these numbers are actually computed, and how reliable they are**, since a performance metric estimated from a short backtest carries a large error that is usually ignored.

The recurring warning of the course applies directly here: for most of the strategies covered so far, **ordinary volatility understates the true risk, because the losses are rare, asymmetric, and concentrated in bad periods**. Choosing the metric is therefore not a formality. It determines whether a strategy that sells hidden tail risk looks good or bad.

Primary reading: Lhabitant, *Handbook of Hedge Funds* (the chapters on risk measurement and performance measurement); Chan, *Algorithmic Trading: Winning Strategies and Their Rationale* (Sharpe ratio, drawdown, and the practical calculation of these metrics); López de Prado, *Advances in Financial Machine Learning* (the reliability of the Sharpe ratio under multiple testing, used in Section 8).

Supplementary reading:
- Bacon, *Practical Portfolio Performance Measurement and Attribution* — the reference for the definitions and annualisation conventions of the ratios in Sections 5 and 6.
- Lo (2002), "The Statistics of Sharpe Ratios" (*Financial Analysts Journal*) — the standard-error formula for the Sharpe ratio used in Section 8; short and readable.
- Bailey and López de Prado (2014), "The Deflated Sharpe Ratio" (*Journal of Portfolio Management*) — the correction for multiple testing in Section 8.
- Ilmanen, *Expected Returns* (a syllabus book) — the chapters on risk and on the historical drawdowns of major strategies, for the interpretation in Section 6.

---

## 1. Setup: returns, compounding, and annualisation

A performance metric is computed from a series of returns.

A **simple (arithmetic) return** over one period is

    r_t = (P_t − P_{t−1}) / P_{t−1}

A **log return** is

    l_t = ln(P_t / P_{t−1})

Log returns add across time, which makes them convenient for aggregation; simple returns add across assets in a portfolio. The two are close for small returns and diverge for large ones. Use one consistently and state which.

Two averages matter and they are not the same. The arithmetic mean return is the average of the per-period returns. The geometric mean is the constant per-period return that reproduces the final wealth:

    geometric mean = (P_end / P_start)^(1/N) − 1

The geometric mean is what an investor actually earns over time, and it is always lower than the arithmetic mean when returns vary. The gap is driven by volatility, and to a good approximation

    geometric ≈ arithmetic − σ² / 2

This term, **σ²/2, is the drag that volatility imposes on compounded wealth**. It is the first reason risk matters directly for return, not only for comfort: two strategies with the same arithmetic mean but different volatility deliver different terminal wealth.

Annualisation converts a metric measured at one frequency to a yearly figure so that strategies traded at different frequencies can be compared. Let P be the number of periods per year: 252 for daily returns (trading days), 52 for weekly, 12 for monthly. Under the assumption that returns are independent across time,

    mean return scales with P:        μ_annual = μ_period × P
    volatility scales with √P:         σ_annual = σ_period × √P

The square-root rule for volatility follows from variances adding when returns are independent. That independence assumption is often false, and Section 8 covers what happens to these numbers when it is.

---

## 2. Volatility and its limits

Volatility can be estimated as standard deviation of returns. For a sample of N returns with mean r̄,

    σ = sqrt( (1/(N−1)) · Σ_t (r_t − r̄)² )

It often is the default risk measure because it is simple, it aggregates cleanly across a portfolio through the covariance matrix, and it is the correct measure when returns are normally distributed. **Under normality the mean and standard deviation describe the whole distribution**, so a single number captures all the risk.

The problem is that **financial returns are not normal**, and the ways they depart from normality are exactly the ways that matter for risk. Three departures recur.

**Fat tails.** Large moves happen far more often than a normal distribution predicts. A daily equity move of five standard deviations should occur roughly once in ten thousand years under normality; in practice it occurs every few years. Volatility, fitted mostly to the many small returns in the middle of the distribution, understates the size and frequency of the rare large ones.

**Skew.** Many strategies have asymmetric return distributions: many small gains and occasional large losses, or the reverse. Volatility treats an upside deviation and a downside deviation of the same size identically, so it cannot distinguish a strategy that jumps up occasionally from one that crashes occasionally. For an investor these are opposite properties, and several strategies in this course (merger arbitrage, option writing, short-volatility trades) have precisely the harmful kind of skew.

**Clustering and dependence.** Volatility is not constant; calm periods and turbulent periods cluster. Losses also tend to arrive together and in bad states of the world. A risk measure computed as a single unconditional number over the whole sample averages over this and hides the concentration.

The practical consequence is that **volatility is a lower bound on the risk of the strategies in this course, not a complete description**. The next two sections add measures that target the part of the distribution volatility misses.

---

## 3. Downside and tail risk measures

The measures here **focus on losses rather than on dispersion in both directions.**

**Downside deviation (semideviation)** is a standard deviation computed only from returns below a target τ. The target is often zero or the risk-free rate.

    σ_down = sqrt( (1/N) · Σ_t min(r_t − τ, 0)² )

Returns above the target contribute zero. This answers the objection that volatility penalises upside moves: downside deviation penalises only the outcomes an investor dislikes. Note the denominator convention varies between sources (some divide by N, some by the count of below-target observations); state which you use, because the choice changes the number.

**Value at Risk (VaR)** is a threshold loss. The VaR at confidence level α (for example 95%) is the loss that is not exceeded with probability α over the chosen horizon. Estimated from historical returns, it is the negative of the lower quantile:

    VaR_α = − quantile_{1−α}(r)

expressed as a positive loss. A one-day 95% VaR of 2% means that on 95% of days the loss is smaller than 2%, and on the remaining 5% it is larger. VaR is widely used and reported, and it has one serious flaw: **it says nothing about how large the loss is once the threshold is breached.** Two strategies can have the same VaR while one loses slightly more than the threshold in the bad 5% and the other loses everything.

Conditional Value at Risk (CVaR), also called **expected shortfall**, fixes that flaw by averaging the losses in the tail beyond the VaR:

    CVaR_α = − E[ r | r ≤ quantile_{1−α}(r) ]

CVaR (ES) is the average loss on the worst (1−α) fraction of days. It uses information about the size of the tail losses, not only their frequency, so it is the more informative of the two for the fat-tailed, skewed strategies of this course. It also has better mathematical properties as a risk measure (in particular it rewards diversification consistently, which VaR does not always do).

These three measures form a progression. **Downside deviation reweights the standard-deviation calculation toward losses; VaR picks out a point in the loss tail; CVaR describes the average severity of that tail.**

---

## 4. Drawdown

**Drawdown measures loss from a previous peak** rather than dispersion of period returns. It is the risk measure that corresponds most closely to what causes investors and funds to **stop trading a strategy, because it describes a sustained decline in accumulated wealth**, not the scatter of individual days.

Build the cumulative wealth series from the returns:

    W_t = Π_{s ≤ t} (1 + r_s)

Track the running peak, the highest wealth reached up to each point:

    M_t = max_{s ≤ t} W_s

The drawdown at time t is the current wealth relative to that peak:

    DD_t = (W_t − M_t) / M_t          (always ≤ 0)

**Plotting DD_t over time gives the underwater curve**: it is zero at every new high and dips below zero during declines. From it come three numbers.

**Maximum drawdown (MDD)** is the deepest point of the underwater curve, the worst peak-to-trough loss over the sample:

    MDD = min_t DD_t

**Drawdown duration** is the length of time spent below a previous peak, from the start of a decline to the recovery of the old high. A strategy can have a modest maximum drawdown but spend years underwater, which is often the harder property to sit through.

**Time to recovery** is the portion of the duration measured from the trough back to the old peak.

Drawdown captures the path dependence that period-return measures ignore. A sequence of small losses in a row produces a large drawdown even if no single return is extreme, and this ordering information is invisible to volatility, which is unchanged if the same returns are shuffled.

The main statistical caution is that maximum drawdown is an extreme-value statistic estimated from a single sample path. It is the single worst outcome that happened to occur, so it is noisy and it can only grow as the sample lengthens. A longer backtest almost always shows a larger maximum drawdown, which does not mean the strategy became riskier. Compare drawdowns only over equal-length samples.

---

## 5. Risk-adjusted ratios

A risk-adjusted ratio divides a return by a risk measure so that strategies taking different amounts of risk can be compared on a common scale. Each ratio in this section pairs an excess return with one of the risk measures already defined.

**The Sharpe ratio** divides the average excess return by its volatility:

    Sharpe = (r̄ − r_f) / σ

where r_f is the risk-free rate over the same period (often zero). It is the return earned per unit of total volatility, and it is the default performance metric across the industry. Annualise it by annualising the numerator and denominator; under the independence assumption of Section 1 this multiplies the per-period Sharpe by √P. A daily Sharpe of 0.1 corresponds to an annual Sharpe of about 0.1 × √252 ≈ 1.6, which is why the same strategy can be quoted with very different Sharpe numbers depending on the frequency and whether it was annualised.

**The Sharpe ratio inherits every limitation of volatility from Section 2. Because it uses total volatility as the denominator, it penalises upside and downside symmetrically and ignores skew and fat tails entirely**. The following ratios substitute a different risk measure to address specific weaknesses.

**The Sortino ratio** replaces volatility with downside deviation:

    Sortino = (r̄ − τ) / σ_down

It rewards a strategy for upside volatility instead of penalising it, and it is the appropriate ratio when the return distribution is asymmetric and the investor cares only about losses.

**The Calmar ratio** (also called the MAR ratio) divides the annualised return by the maximum drawdown:

    Calmar = annualised return / |MDD|

It measures return per unit of worst-case peak-to-trough loss, and it is the ratio most used by managed-futures and trend-following funds, where surviving the largest drawdown is the binding constraint. It shares the noisiness of maximum drawdown and should be compared only across equal-length samples.

**The Information ratio** measures performance relative to a benchmark rather than to the risk-free rate. Let b_t be the benchmark return and define the active return r_t − b_t. Then

    Information ratio = mean(r_t − b_t) / std(r_t − b_t)

The denominator, the standard deviation of the active return, is called the tracking error. The Information ratio is the right measure when a strategy is meant to beat a benchmark rather than to produce absolute return, and it is standard for evaluating active managers against an index.

These four cover the common cases. Each is a return divided by a risk measure, and the choice of which one to report is the choice of which risk measure is relevant, which is the subject of the next section.

---

## 6. Which metrics matter most

**There is no single correct metric**. The right one depends on the strategy's return distribution and on what the investor is trying to avoid. Three principles guide the choice.

Match the risk measure to the shape of the losses. If the return distribution is roughly symmetric, volatility describes it well and the Sharpe ratio is adequate. If the distribution is skewed, with occasional large losses, volatility understates the risk and the Sharpe ratio flatters the strategy; report the Sortino ratio and a tail measure (CVaR) alongside it. If the binding concern is a sustained decline in capital rather than day-to-day scatter, drawdown and the Calmar ratio are the relevant measures.

Understand what the Sharpe ratio can be made to hide. Because the Sharpe ratio uses only the mean and the volatility, a strategy can raise it by selling tail risk: writing options, running short-volatility positions, or any trade that collects a small steady premium in exchange for a rare large loss. Such a strategy shows many small gains, which lowers measured volatility and raises the Sharpe ratio, while the rare loss that defines its true risk does not appear in the sample until it happens. This is not a hypothetical; it is the standard failure mode of strategies that report high Sharpe ratios and then lose most of their value in a single episode. A high Sharpe ratio combined with negative skew is a warning, not a recommendation. This is the concrete form of the course's recurring point that volatility understates the risk of these strategies.

Report a small set, not one number. Because each ratio captures a different aspect, the practice is to report several: a Sharpe ratio for the headline, a Sortino ratio to expose asymmetry, a maximum drawdown and Calmar ratio for the path, and the higher moments (skewness and kurtosis) of the return distribution so the reader can see directly whether the Sharpe ratio is trustworthy. A strategy that looks good on all of these is more convincing than one that looks good on the single metric its author chose to show.

The general lesson is that the metric is part of the claim. A performance number reported without stating the risk measure, the frequency, the annualisation convention, and whether it is gross or net of costs is not interpretable, and the omission is often where a weak strategy is made to look strong.

---

## 7. Calculating the metrics in practice

The calculations are short. The errors are almost all in the conventions and the inputs, not in the formulas.

A standard workflow on a return series is: compute the periodic returns from prices; subtract the risk-free rate to form excess returns where the metric requires it; compute the mean, volatility, downside deviation, skewness, and kurtosis; build the cumulative wealth and underwater curves and read off the maximum drawdown and its duration; estimate VaR and CVaR from the empirical quantiles of the return series; then form the ratios of Section 5 and annualise them consistently.

Four conventions decide whether two people's numbers agree.

Frequency and annualisation. State the base frequency and use the matching P (252, 52, or 12). Do not mix, for example computing volatility from daily data and mean return from monthly data.

Gross versus net. Metrics computed before transaction costs overstate performance, and by more for high-turnover strategies, as established in Lessons 5 to 7. Report net figures, and report turnover alongside so the reader can see how large the cost adjustment was.

Risk-free rate. The excess return in the Sharpe and Sortino ratios is relative to a risk-free rate over the same horizon. Using zero when rates are high, or an annual rate against daily returns without converting, produces meaningfully wrong ratios.

Benchmark choice. The Information ratio depends entirely on the benchmark. Choosing an easy benchmark inflates it, so the benchmark must match the strategy's asset class and risk (the point returns in Lesson 11 for alternative assets, where an appropriate benchmark is often unavailable).

The output of this step is a small table of metrics that is then used to compare strategies. Every later lesson that reports performance (crypto in Lesson 9, algorithmic strategies in Lesson 10, alternative assets in Lesson 11) uses this same table.

---

## 8. How reliable are these numbers

A performance metric is an estimate from a finite sample, and it carries an error that is usually left unstated. For short backtests the error is large enough that the reported metric can be consistent with the strategy having no real edge at all.

The Sharpe ratio has a known standard error. For returns that are independent and roughly normal, Lo (2002) gives the approximate standard error of an estimated Sharpe ratio over N periods as

    se(Sharpe) ≈ sqrt( (1 + 0.5 · Sharpe²) / N )

The point of the formula is the dependence on N. A Sharpe ratio of 1 estimated from one year of daily data (N ≈ 252) has a standard error near 0.07, so it is measured reasonably well. The same Sharpe of 1 estimated from one year of monthly data (N = 12) has a standard error near 0.35, so the true value could plausibly be anywhere from roughly 0.3 to 1.7. A strong-looking Sharpe ratio from a short monthly sample is often statistically indistinguishable from zero.

Autocorrelation inflates the Sharpe ratio. The √P annualisation in Section 1 assumes returns are independent across time. Strategies that hold illiquid positions or smooth their marked prices (common in some hedge-fund and alternative-asset returns) show positively autocorrelated returns, which makes the measured volatility too low and the annualised Sharpe ratio too high. The correction reduces the ratio and is standard when evaluating monthly hedge-fund returns.

Multiple testing inflates the best result. Every strategy in this course was one of many that were tried, as the backtesting sections of Lessons 5 to 7 stressed. If a hundred variants are tested and the best is reported, its Sharpe ratio is biased upward by selection alone, even if none of the variants has a real edge. The deflated Sharpe ratio (Bailey and López de Prado, 2014) adjusts the observed Sharpe ratio for the number of trials and the non-normality of the returns, and gives the probability that the true Sharpe ratio is above zero. The practical rule is to record how many configurations were tested and to treat a headline metric selected from many as provisional.

The conclusion for the course is that a performance metric should be reported with its sample length, its frequency, and the number of variants tested, and that a metric from a short or heavily searched backtest should be treated as an upper bound on the strategy's real performance rather than an estimate of it.

---

## Home tasks

Use free data (price data from yfinance or Stooq; a strategy return series you generated in an earlier lesson is a good input). Where a task refers to an earlier strategy, you can reuse your own code from that lesson.

1. Full metric table for one strategy.
   Take one return series (a strategy from Lesson 5 or 6, or a single asset). Compute the complete set of metrics from this lecture: annualised return and volatility, Sharpe, Sortino, Calmar, maximum drawdown and its duration, and the 95% VaR and CVaR. Present them as one table and state every convention you used (frequency, risk-free rate, gross or net).
   Stretch: compute the Lo (2002) standard error of the Sharpe ratio for your sample, and state whether the Sharpe ratio is statistically distinguishable from zero at your sample length.

2. Volatility understates risk.
   Construct or obtain two return series with similar Sharpe ratios but different skew, for example a broad equity index and a covered-call (short-volatility) version of it. Compute the Sharpe ratio, Sortino ratio, skewness, and CVaR for both, and show that the ratios rank the two strategies differently depending on which risk measure is used.
   Stretch: identify which single metric most clearly exposes the hidden tail risk in the short-volatility series, and explain why, linking the result to the skew argument of Section 6.

3. Drawdown analysis.
   For one asset or strategy over at least ten years, build the cumulative wealth and underwater curves, and report the maximum drawdown, its start and trough dates, and the time to recovery. Plot the underwater curve.
   Stretch: compute the full distribution of drawdown durations (not only the maximum), and compare the Calmar ratios of two assets, noting how the comparison depends on the length of the sample you used.

4. Metric sensitivity to frequency and dependence.
   Take one daily return series and compute its annualised Sharpe ratio three ways: from daily data, from the same returns aggregated to weekly, and aggregated to monthly. Report the three numbers and explain why they differ.
   Stretch: measure the first-order autocorrelation of the daily returns, and apply the autocorrelation correction from Section 8 to the annualised Sharpe ratio. Report the corrected figure and state whether the original overstated the strategy.

5. Re-evaluation of an earlier strategy net of costs.
  Take a strategy you built in Lesson 5 or 6, recompute its performance net of transaction costs, and report the full metric table (as in task 1) for both the gross and net versions. State how much the cost adjustment changed each metric.
   Stretch: record how many parameter configurations of that strategy you tried in total, and use that count to argue, following the deflated-Sharpe reasoning of Section 8, whether the net Sharpe ratio you report is likely to survive out of sample.
