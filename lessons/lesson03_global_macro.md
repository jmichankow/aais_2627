# Lesson 3: Global Macro Strategies (Rates, Bonds, the Yield Curve)

This lecture introduces global macro, a strategy that trades on views about whole economies. Its main instruments are government bonds and interest-rate derivatives. Most of the lecture covers how a macro desk works with them: how central banks set rates, how bond prices react to rate changes (duration and convexity), how to read the yield curve, and how a view on the economy becomes a trade. We also look at the ways these trades fail.

Primary reading: Ilmanen, *Expected Returns* (rates, bond risk premium, carry); Lhabitant, *Handbook of Hedge Funds* (global macro / managed futures chapters).

---

## Bonds introduction

A bond is a list of future payments, and its price is what those payments are worth today. Work through this section with a spreadsheet before reading further.

A bond works like a loan. You lend money (the face value, for example €100). The borrower pays you interest each year (the coupon, for example 5% of face value, or €5) and returns the €100 at the end (maturity).

To price the bond, you discount each payment to today and add the results. Discounting reflects the fact that €1 received in the future is worth less than €1 today, because money today can earn interest. You divide each payment by `(1 + y)^t`, where `t` is the number of years until the payment and `y` is the yield, the interest rate used for discounting.

Example: a 3-year bond, €100 face value, 5% annual coupon, priced at a yield of 6%.

| Year (t) | Cash flow | Discount factor `1/(1+0.06)^t` | Present value |
|---|---|---|---|
| 1 | 5 | 0.9434 | 4.717 |
| 2 | 5 | 0.8900 | 4.450 |
| 3 | 105 | 0.8396 | 88.157 |
| | | Price = | **97.32** |

What the table shows:

- The price (97.32) is below the face value (100) because the yield (6%) is higher than the coupon (5%). When the yield rises, the price falls. When the yield falls, the price rises. A trader who expects yields to fall therefore expects bond prices to rise, and buys bonds.
- The yield to maturity is the value of `y` that makes the sum equal to the market price. There is no formula that gives it directly, so it is found numerically (YIELD or IRR in a spreadsheet, a root-finder such as `scipy.optimize.brentq` in Python). In practice the market sets the price, and the yield is calculated from it.
- About 90% of the value comes from the last payment, because it includes the €100 principal. Payments far in the future lose more value when the yield changes. This is why long bonds react more strongly to rates.

Duration, informally. Raise `y` slightly and recompute the table: the price falls. Duration measures how much it falls, expressed as the percentage price change for a 1% change in yield. Bonds whose payments arrive later have higher duration, so long-maturity and low-coupon bonds are the most sensitive. Section 4 gives the exact definition. Section 5 adds a correction (convexity) for large yield changes.

If you can rebuild this table in a spreadsheet and see the price change when you change `y`, you have the background needed for the rest of the lecture.

---

## 1. Global macro: the strategy

Global macro is a top-down strategy. The manager forms a view on the economy (growth, inflation, central bank policy, flows of trade and capital between countries) and trades the markets that should react to it. In long/short equity (Lesson 2) the unit of analysis is a single company. In global macro it is a country or a whole asset class.

The industry has two main groups:

- Discretionary macro. A portfolio manager forms a view by judgement, for example: "inflation is higher than the Fed expected, so it will raise rates faster than the market prices, and short-term yields will rise more than long-term yields". The manager then picks the cheapest instrument with the largest payoff if the view is right. Known examples: Soros and Druckenmiller (the bet against the British pound in 1992), Bridgewater (Ray Dalio), Brevan Howard, Rokos. They hold a few large positions, chosen so that the possible gain is much larger than the possible loss.
- Systematic macro, also called CTA or managed futures. Computer rules generate the trades. The two most common rules are trend (buy markets that have been rising, sell those that have been falling) and carry (prefer assets that pay you to hold them). The rules run across dozens of futures markets at once. Known examples: Winton, AQR, Man AHL. These funds are diversified and have low correlation with stocks. Trend-following funds often gain during stock market crashes, which the industry calls "crisis alpha".

The instruments macro traders use:

| Asset class | Primary instrument | View expressed |
|---|---|---|
| Rates | Govt bond futures, swaps, STIR futures (SOFR, €STR) | Level and shape of the curve, policy path |
| FX | Spot, forwards, options | Relative growth/policy, carry |
| Equity | Index futures | Risk-on / risk-off, regional growth |
| Commodities | Futures | Inflation, supply/demand cycles (Lesson 4) |
| Credit | CDS indices, cash bonds | Cycle stage, default risk |

STIR futures are short-term interest rate futures. A CDS (credit default swap) is a contract that pays out if a borrower defaults, similar to insurance.

Rates are the centre of the strategy for three reasons. They are the most liquid markets, they allow high leverage, and they react most directly to central banks, which every macro trader follows. The rest of the lesson covers rates.

---

## 2. The Fed and the policy rate

The central bank sets the shortest interest rate in the economy: the overnight rate at which banks lend to each other. All other rates and asset prices are set in relation to it. A macro trader therefore starts by forecasting what the central bank will do.

How it works:

- Mandate. The Fed has two goals: maximum employment and stable prices (2% inflation). The ECB has one goal: stable prices. Because their goals differ, the two banks can react differently to the same event, such as a slowdown. A trader can bet on this difference, for example by buying German bonds and selling US bonds.
- Setting the rate. The Fed announces a target range for the overnight rate. It keeps market rates inside this range mainly by paying banks interest on the money they hold at the Fed (interest on reserve balances). A bank will not lend to another bank for less than it earns at the Fed, so this rate works as a floor.
- Transmission. A change in the policy rate moves other short-term rates, then loan and mortgage rates, then the discount rates used to value all assets. The effect on the real economy arrives with a delay of about 12 to 18 months (Friedman's "long and variable lags").

Modelling the central bank. Traders describe the central bank's behaviour with a rule. The standard starting point is the Taylor rule:

    i = r* + π + 0.5(π − π*) + 0.5(output gap)

- `i`: the policy rate the rule recommends
- `r*`: the neutral real rate, the inflation-adjusted rate that neither speeds up nor slows down the economy
- `π`: current inflation; `π*`: the inflation target
- output gap: how far output is above (+) or below (−) its sustainable level

No central bank follows the rule exactly. For a trader, the useful information is the gap between the rule and the actual rate. If the actual rate is below the rule, the bank is dovish (it accepts more inflation to support growth). If the actual rate is above the rule, the bank is hawkish. Changes in this gap can be traded.

Reading expectations from prices. Market prices already contain a forecast of the policy rate:

- Fed funds futures and SOFR futures give the expected average policy rate for each future month or quarter.
- The OIS (overnight index swap) curve gives the same information for longer horizons.

A trade makes sense only when your forecast differs from the market's. If you expect two rate cuts this year and the market already prices two cuts, there is nothing to trade.

When rates are at zero. Once the policy rate reaches zero (the effective lower bound), central banks use two other tools. Forward guidance means announcing future policy in order to move long-term rates today. Quantitative easing (QE) means buying long-term bonds, which raises their prices and lowers their yields. Quantitative tightening (QT) reverses this. In the 2013 "taper tantrum", the Fed only hinted that it would slow its bond purchases, and yields across the whole curve rose sharply.

---

## 3. The business cycle and sector rotation

Economies move through repeating phases of growth and decline. Macro traders try to identify the current phase and position for the next one. The current phase is already reflected in prices, so profits come from anticipating the change.

| Phase | Growth / inflation | Rates view | Equity leadership |
|---|---|---|---|
| Early expansion (recovery) | Growth ↑, inflation low | Curve steep, cuts done | Cyclicals, small caps, financials |
| Mid expansion | Growth strong | Curve flattening, hikes begin | Broad market, industrials, tech |
| Late expansion | Growth peaking, inflation ↑ | Flat/inverting, Fed tight | Energy, materials, staples begin |
| Recession | Growth ↓ | Curve re-steepens, cuts | Defensives: utilities, staples, healthcare |

Sector rotation. Different stock market sectors perform best in different phases. Cyclical sectors (banks, industrials, small companies) depend on strong growth. Defensive sectors (utilities, food and household goods, healthcare) sell products people buy in any economy. This covers the "industries" topic of the syllabus and connects the stock analysis of Lesson 2 with the macro view: the cycle tells you which sectors to prefer.

Economic indicators grouped by timing:

- Leading indicators change before the economy does, so they are the most useful for trading: the slope of the yield curve (Section 6), ISM/PMI surveys of new orders, building permits, new unemployment benefit claims, and corporate credit spreads (the extra yield on company bonds over government bonds).
- Coincident indicators move together with the economy: payroll employment, industrial production, real income.
- Lagging indicators change after the economy: the unemployment rate, CPI inflation, labour costs. They confirm what has already happened. Markets have already priced this information, so lagging data is a poor basis for a trade.

Systematic approach. A model can replace judgement in classifying the economy. A simple version uses four boxes: growth rising or falling, combined with inflation rising or falling. The portfolio holds the assets that historically did best in the current box. Risk-parity funds and Bridgewater's "All Weather" portfolio are based on this idea.

---

## 4. Bond pricing: price, yield, duration

Section 0 showed that bond prices fall when yields rise. This section measures how strongly.

Four yield measures that are often confused:

- Coupon rate: the fixed interest rate set when the bond was issued. After issue, it says little about the return a new buyer will earn.
- Current yield: annual coupon divided by price. A rough measure that ignores the repayment of principal.
- Yield to maturity (YTM): the discount rate that makes the present value of all payments equal to the market price (the `y` from Section 0). This is the standard quoted yield.
- Spot rate (zero rate): the yield on a single payment at one date, with no coupons in between. Spot rates are the basic building blocks for valuing any bond (Section 6).

Three duration measures:

- Macaulay duration: the average time until you receive the bond's payments, in years. Each payment is weighted by its share of the bond's price. A 10-year zero-coupon bond has one payment, so its Macaulay duration is 10 years. A 10-year coupon bond has a shorter duration, because the coupons arrive earlier.
- Modified duration: the percentage price change for a 1 percentage point (100 basis points) change in yield. `ModDur = MacDur / (1 + y/n)`, where `n` is the number of coupon payments per year. With a modified duration of 7, a 1% rise in yield lowers the price by about 7%. One basis point (bp) equals 0.01%.
- DV01 (also called PV01): the change in a position's value, in currency, for a 1 bp change in yield. Trading desks measure and hedge risk in DV01, because it allows positions in different bonds to be compared and added up. To hedge a bond with a futures contract, size the futures position so that both have the same DV01 with opposite signs. A small yield move then produces a gain on one side and an equal loss on the other.

Duration treats the relationship between price and yield as a straight line. The actual relationship is curved. For small yield changes the straight line is accurate. For large changes the error grows. Section 5 corrects for this.

---

## 5. Convexity

Convexity measures the curvature of the price–yield relationship. It shows how much duration itself changes when yields move.

Intuition. Draw the price–yield curve, and the straight line given by duration that touches the curve at today's yield. Close to today's yield, the line and the curve almost coincide. Further away, they separate. Convexity measures this gap and serves as the correction to the duration estimate. For an ordinary bond the correction always works in the holder's favour.

Practical consequences:

- Positive convexity (ordinary government bonds). When yields fall, the price rises more than duration predicts. When yields rise, the price falls less than duration predicts. In both cases the holder gains from convexity. Long-maturity and low-coupon bonds have more convexity.
- Hedged positions still carry convexity. Suppose you hold a bond with high convexity and hedge it with an instrument of the same duration but lower convexity. Small yield moves now cancel out, but large moves in either direction produce a profit. The position behaves like a bet on volatility, and desks build it on purpose when they expect large moves.
- Negative convexity. Some bonds behave the opposite way. The main examples are callable bonds (the issuer can repay early) and mortgage-backed securities (MBS, bonds backed by pools of home loans). When rates fall, homeowners refinance and repay their mortgages early, so the MBS becomes shorter and loses duration exactly when duration would gain value. When rates rise, refinancing stops and the MBS becomes longer. MBS holders hedge this by selling bonds when rates rise and buying when rates fall. This pushes rates further in the direction they are already moving, and it is a regular source of volatility in US Treasuries, especially in the middle of the curve.

Formula (second-order approximation):

    ΔP/P ≈ −ModDur × Δy + ½ × Convexity × (Δy)²

The first term is the duration effect. The second term is the convexity correction. For ordinary bonds it is positive whichever way yields move, because `(Δy)²` is always positive. When a trade is sized for a large expected yield move, leaving out this term gives the wrong position size.

---

## 6. Term structure and the yield curve

The yield curve shows yields for different maturities at one point in time, for example the 3-month, 2-year, 10-year and 30-year government bond yields. Two questions matter: how the curve is built, and why it has its shape.

Building the curve:

- Par curve: yields of bonds that trade at their face value. This is the curve usually quoted.
- Spot (zero) curve: the yield for a single payment at each maturity. It is derived from the par curve by bootstrapping: find the shortest spot rate first, use it to find the next one, and continue step by step to longer maturities. The spot curve is the correct curve for discounting.
- Forward curve: the rates for future periods implied by today's spot curve. Example: with the 1-year spot rate `z₁` and the 2-year spot rate `z₂`, the equation `(1+z₂)² = (1+z₁)(1+f)` gives `f`, the 1-year rate one year from now. Forward rates show what the market expects future short rates to be. A trader who expects a different future rate trades against the forward.
- Nominal and real curves: inflation-linked bonds (TIPS in the US) pay a yield above inflation, which gives the real curve. Nominal yield minus real yield is breakeven inflation, the market's implied inflation forecast. Trading breakevens is a direct bet on inflation.

Why the curve has its shape. Three explanations:

- Expectations theory: long-term rates equal the average of expected future short-term rates. If this held exactly, forward rates would be unbiased forecasts. The data mostly reject it.
- Term premium: investors require extra yield for holding long bonds, because their prices move more. Long yield = expected average short rate + term premium. Most practitioners use this view. The term premium cannot be observed directly and must be estimated with a model, most often the ACM model (Adrian, Crump and Moench) published by the New York Fed. Different models give different estimates.
- Market segmentation (preferred habitat): some investors prefer particular maturities. Pension funds buy long bonds to match their long-term obligations; banks prefer medium maturities. Supply and demand at one maturity can therefore move that part of the curve on its own. This explains why QE and changes in government bond issuance affect specific maturities.

Level, slope, curvature. Principal component analysis (PCA) of daily yield changes shows that three factors explain about 95% of all movements of the curve:

- Level: all yields move up or down together (a parallel shift). This is duration risk.
- Slope: short and long yields move apart. The curve steepens or flattens.
- Curvature: the middle of the curve (the belly) moves relative to the short and long ends (the wings).

Each trade in Section 7 takes a position on one of these factors and hedges the other two.

---

## 7. Yield-curve trades

Each trade below targets one of the three factors from Section 6.

- Outright duration (level). Buy or sell bonds because you expect rates in general to fall or rise. Position size is measured in DV01. This is the simplest macro trade and also the most crowded.
- Steepener or flattener (slope). Buy one maturity and sell another, typically the 2-year against the 10-year ("2s10s"). The two legs are sized to have equal DV01, so a parallel move in rates produces no gain or loss. Only a change in the spread between the two yields affects the result.
  - Bull steepener: short-term yields fall faster than long-term yields, because the market expects rate cuts. Typical at the start of a recession.
  - Bear flattener: short-term yields rise faster than long-term yields, because the central bank is raising rates. Typical late in an expansion.
- Butterfly (curvature). Buy the wings and sell the belly, or the reverse, sized to be neutral to both level and slope. The trade profits if the middle of the curve becomes cheaper or more expensive relative to the ends. A standard version is the 2s5s10s butterfly.
- Cross-country relative value. Buy one country's bonds and sell another's, for example buy German Bunds and sell US Treasuries, because you expect the two central banks to act differently. A global move in rates affects both legs and largely cancels out.
- Breakeven inflation. Buy inflation-linked bonds and sell nominal bonds (or use inflation swaps). The trade profits if inflation turns out higher than the market priced.

Carry and roll-down. Part of the return comes even when rates do not move. Carry is the coupon income minus the cost of financing the position. Roll-down arises when the curve slopes upward: after one year, a 10-year bond becomes a 9-year bond. If the curve has not changed, the 9-year yield is lower, so the bond's price is higher. A large part of systematic rates returns comes from carry and roll-down, without any forecast of rate direction. Ilmanen treats carry as a reward for bearing risk that any investor can collect. It should be measured separately from forecasting skill (the alpha versus beta question from Lessons 1 and 2).

The yield curve as a recession signal. The curve is inverted when short-term yields are above long-term yields, measured by a negative 2s10s or 3m10y spread. Every US recession since the 1960s was preceded by an inversion, with few false signals. This is why the inversion is so widely watched. Its limitations in practice:

- The time between inversion and recession has varied from about 6 to 24 months, so the signal says a recession is likely without saying when.
- The depth of the inversion does not predict how severe the recession will be.
- QE has lowered term premia, which makes the signal in recent decades less reliable than the long historical record suggests.

Many traders therefore wait for the curve to steepen again after the inversion ("un-inversion"), which often coincides with the start of the recession.

---

## 8. Practical pitfalls

- The term premium cannot be observed. A trade based on "the term premium is too low" depends on the model that produced the estimate. Name the model and state its uncertainty.
- Leverage and financing. Rates trades use high leverage through futures and repo (short-term borrowing secured by bonds). Leverage multiplies losses as well as gains, and the repo rate can rise independently of the trade. Examples: the repo rate spike of September 2019, the "dash for cash" of March 2020, and the UK pension (LDI) crisis of 2022, when leveraged positions in long-dated UK government bonds were sold in a rush and the Bank of England had to intervene.
- Carry trades have negative skew. They produce small steady gains and occasional large losses when the trade reverses, similar to selling insurance. Set position size based on the possible large loss rather than the average return.
- Trading against the central bank. A central bank committed to a policy can hold it for years. Selling Japanese government bonds while the Bank of Japan capped their yields lost money for so long that traders called it "the widowmaker". A view can be correct and still lose money because of timing.
- Data revisions and lags. Employment and GDP figures are often revised substantially after the first release. Trading heavily on the first release, or on lagging indicators, is a common mistake.
- Crowded signals. Everyone watches curve inversion and trend signals. Noticing the signal gives no advantage; any advantage comes from timing and position size.

---

## Additional reading (for this lesson)

Bodie, Kane & Marcus, *Essentials of Investments* — Ch. 10 - Ch. 12.
Very good intro level bonds in general, managing bonds and macro cycles.

## 9. Reading assignment (next lessons topic)

Papers (open source):

- Neville, Draaisma, Funnell, Harvey & Van Hemert (2021), "The Best Strategies for Inflationary Times" — a direct, data-driven study of what actually hedged inflation across the historical episodes. [Link](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3813202)

- Gorton & Rouwenhorst (2006), "Facts and Fantasies about Commodity Futures" — the classic on commodity-futures total return, the roll, and the equity-diversification case. [Link](https://www.nber.org/papers/w10595)

Core (course list):
 
- Ilmanen, *Expected Returns* — the chapters on inflation, real assets, and commodities. The best single treatment of inflation betas, the expected-vs-unexpected distinction, and commodity return decomposition.
- Ang, *Asset Management* — the real-assets and macro-factor chapters, for the factor view of inflation and the treatment of commodities and real estate as macro exposures.

Supplementary (stronger on specific points than the core list):
 


- Erb & Harvey (2006), "The Strategic and Tactical Value of Commodity Futures" — the definitive dismantling of the "spot return = commodity return" fallacy, and the case for roll (carry) as the real driver. [Link](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=650923)

---

## 10. Home tasks

Deliver short report (5 pages max) + reproducible code/spreadsheet via email (one email per team). Deadline by the end of Monday.

All tasks use free data:

- FRED, the economic database of the St. Louis Fed, accessed in Python with `pandas-datareader`
- daily Treasury yield files (CSV) from the US Treasury website
- `yfinance` for prices of futures and ETFs
- the New York Fed term-premium series (ACM model, Section 6)

1. Build and decompose the yield curve.
   Download daily US Treasury yields from FRED for maturities from 3 months to 30 years, covering at least 15 years. These "constant-maturity" yields already form a yield curve, so no bootstrapping is needed.
   Compute the daily change in yield for each maturity and run PCA on these changes. Show that the first three components correspond to level, slope and curvature (Section 6), and report the share of total variance each one explains. Plot the loadings of the three components, which show how strongly each maturity reacts to each component. Check whether the three components together explain about 95% of the variance.
   *Optional extension:* for one recent date, bootstrap the spot (zero) curve from the par curve and compare it with the constant-maturity yields.

2. Curve-inversion recession signal.
   Download two spreads from FRED, starting as early as the data allow (the 1960s or 1980s): the 2s10s spread (10-year yield minus 2-year yield) and the 3m10y spread (10-year yield minus 3-month yield). Mark the official US recession dates (NBER, also available on FRED).
   For each inversion (spread below zero), measure the number of months until the next recession started. Count the false signals, meaning inversions that no recession followed.
   Then compare two simple rules. Rule A moves the portfolio from stocks to Treasury bills when the curve inverts. Rule B waits until the curve is no longer inverted. Compare the returns of the two rules. In the report, discuss how reliable the signal is and how much the lead time varies.

3. Taylor rule vs actual policy.
   For each month since about 1990, calculate the policy rate recommended by the Taylor rule (Section 2). Use FRED data: core PCE inflation for `π`, and the unemployment gap or the output gap for the state of the economy. Choose values for `r*` (for example 1% to 2%) and `π*` (2%), and state them in the report.
   Plot the Taylor-rule rate against the actual fed funds rate. Find the periods with the largest gap between the two and link them to known episodes, for example the near-zero rates after 2008 and the Fed's late reaction to inflation in 2021–22. Explain how a trader could have positioned for each gap.

4. Duration, convexity, and a DV01 hedge.
   Core part: choose a coupon bond, or define one (for example 10 years, 4% annual coupon). Build its cash-flow table as in Section 0. Compute its price, Macaulay duration, modified duration and DV01. Then change the yield by ±25, ±100 and ±300 bp, reprice the bond exactly each time, and compare the true price change with the estimate from duration alone (`−ModDur × Δy`). Show at what size of yield change the duration estimate becomes inaccurate.
   Extension: add the convexity term and check that `ΔP/P ≈ −ModDur × Δy + ½ × Convexity × (Δy)²` removes most of the error at ±300 bp. Then hedge the bond with a Treasury futures contract (or a proxy), sized so that the bond and the hedge have the same DV01. Report the remaining profit or loss of the hedged position under the same yield changes.

5. Steepener/flattener backtest with carry and roll.
   Build a 2s10s steepener: buy the 2-year and sell the 10-year, sized so that both legs have the same DV01 (Section 7). The steepener gains when the spread between the two yields widens; the opposite position (the flattener) gains when it narrows. Use two FRED constant-maturity yield series, or two Treasury ETFs as proxies (one with short maturity, one with long maturity).
   Backtest the position over at least 10 years. Split its profit and loss into two parts: the part caused by changes in the 2s10s spread, and the part from carry and roll-down. Report the annualised return, volatility, Sharpe ratio and maximum drawdown (the largest fall from a peak to a later low). Mark the business-cycle phases from Section 3 on your chart and comment on when the steepener or the flattener made money.
