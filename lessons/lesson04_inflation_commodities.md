# Lesson 4 - Inflation and Commodity-Cycle Strategies

In this lecture we will have a look on how inflation reorganizes the entire portfolio problem, and how commodities - the asset class most directly tied to inflation are actually traded. Three threads run through the lesson. First, the distinction between nominal and real returns, and the idea that every asset has a measurable sensitivity to inflation. Second, the role of the US market and the US dollar as the center of gravity for global markets, and the fact that correlations between assets are not fixed numbers but shift with the macro regime. Third, the mechanics of holding commodities, gold and oil in particular, where the return comes from more than just the price of the underlying.


Primary reading: Ilmanen, *Expected Returns* (the chapters on inflation, real assets, and commodities); Ang, *Asset Management* (real assets and the macro-factor view).

---

## 1. Why inflation is the organizing variable

Investors care about purchasing power, not the number of currency units they hold. If a portfolio grows 8% in a year when prices rise 8%, the investor is no better off, they can buy exactly the same basket of goods as before. This is the difference between the **nominal return** (the headline number) and the **real return** (the nominal return adjusted for inflation). The rough relationship is:

    real return ≈ nominal return − inflation

The whole lesson is about real returns. An asset is only useful as a store of wealth if it protects, or grows, purchasing power.

The reason inflation deserves its own lesson is that different assets respond to it in completely different ways, and those differences are large enough to dominate allocation decisions. Some assets lose badly when inflation rises; some are roughly neutral; a few gain. The single number that captures this is the inflation beta.

**Inflation beta** is the sensitivity of an asset's return to inflation. Formally it is the slope coefficient from regressing the asset's return on inflation:

    asset return = α + β · inflation + ε

A positive beta means the asset tends to rise when inflation rises (it hedges inflation). A beta near zero means inflation does not move it much. A negative beta means the asset falls when inflation rises. As a rough empirical ranking, nominal government bonds have a strongly negative inflation beta, equities have a low or slightly negative beta, and commodities have a strongly positive beta. Most of this lecture is about why.

One refinement matters throughout. What moves markets is not the level of inflation but the surprise, the gap between realized inflation and what was already expected and priced in. Expected inflation is largely built into asset prices already: a bond yield of 5% when everyone expects 3% inflation already contains that expectation. It is unexpected inflation, inflation that arrives above what was priced, that reprices assets. So when we talk about inflation hedging, the relevant question is: which assets protect you against inflation you did not see coming?

---

## 2. The US market and the dollar as the global center

Before inflation hedging, one structural fact about global markets. The US is not just one market among many; it sets the conditions for the others. Three channels explain why.

The first is **monetary policy**. US Federal Reserve policy sets the price of the world's reserve currency. When the Fed raises rates, dollar-denominated assets become more attractive, capital flows toward the US, the dollar strengthens, and financial conditions tighten everywhere, including in countries whose own central banks did nothing. This is why a Fed decision moves markets in Warsaw, São Paulo, and Jakarta.

The second is the **dollar funding** channel, which hits emerging markets hardest. Many emerging-market governments and companies borrow in dollars rather than their own currency. When the dollar strengthens, their debt becomes more expensive to service in local-currency terms, even though they did nothing to increase it. A strong dollar is therefore a headwind for emerging markets, and a weak dollar a tailwind. This is one of the most reliable cross-asset relationships in global macro.

The third is risk sentiment. In calm markets, capital spreads out into higher-yielding, higher-risk markets ("**risk-on**"). In stress, it retreats to US Treasuries and the dollar, regardless of where the stress originated ("**risk-off**"). This flight-to-safety means that in a crisis, correlations between risky assets all over the world rise toward one, the diversification you thought you had disappears exactly when you need it.

That last point generalizes into the central warning of this lesson: correlations are not constants. They change with the regime.

Developed markets (DM) and emerging markets (EM) illustrate this. In normal times EM equities offer real diversification and higher expected returns. But EM tends to underperform sharply when the dollar is strong, when US rates rise, and when global growth slows, and its correlation with US equities spikes in exactly those stress episodes. So the diversification EM appears to offer in a backtest run over calm years partly evaporates in the years you were buying it to protect against.

---

## 3. How commodities actually work - return mechanics

Commodities are the asset class with the highest inflation beta, which is why they anchor most inflation-hedging strategies. But holding them is not like holding a stock, and the difference is the single most important practical point in this lesson.

You cannot practically hold most commodities in physical form, storing crude oil or live cattle is not an option for a fund. Instead, exposure is taken through **futures contracts**: agreements to buy the commodity at a fixed price on a future date. Because a futures contract expires, a fund that wants continuous exposure must, before expiry, sell the expiring contract and buy the next one further out. This is called rolling the position, and it is where the unusual return mechanics come from.

The total return of a commodity futures position has three parts:

    total return = spot return + roll return + collateral return

The **spot return** is the change in the commodity's actual price, the part everyone thinks about.

The **roll return** comes from the shape of the futures curve, i.e. whether longer-dated contracts are more or less expensive than nearer-dated ones. Two cases:

- **Contango**: distant contracts are more expensive than near ones. Each roll means selling a cheaper expiring contract and buying a more expensive further-out one, you buy high and sell low every roll. Roll return is negative. This is a persistent drag, and it is why long-term holders of oil ETFs often lose money even when the oil price is flat.
- **Backwardation**: distant contracts are cheaper than near ones. Each roll earns a positive return. Historically, backwardation is associated with commodities in tight supply and is a signal that carry-based commodity strategies exploit.

The **collateral return** is earned on the cash backing the futures position. Because futures require only a small margin, the rest of the capital sits in short-term instruments (e.g. Treasury bills) and earns the risk-free rate. In a high-rate environment this is a meaningful positive contribution.

The practical consequence: a commodity index can have a positive spot return over a decade and still deliver a negative total return if it spent that decade in contango. **Any backtest that uses spot prices instead of a total-return futures index is measuring something that no investor could actually have earned**. This is the most common error in commodity backtests.

### Gold and oil are different animals

The two commodities in the syllabus behave in almost opposite ways, and it is worth separating them.

**Oil** is a growth-and-inflation asset. Its price is driven by the balance of global demand (which rises with economic growth) and supply (OPEC decisions, shale production, geopolitics). It tends to rise when the economy is running hot and inflation is building, and to collapse in recessions. Its inflation beta is high, but it carries heavy growth risk alongside it.

**Gold** is a monetary asset, not really an industrial one. It produces no cash flow, no yield, and has little industrial demand relative to its price. Its value comes from being a store of wealth that no government can print. The cleanest driver of the gold price is the real interest rate, the interest rate after subtracting inflation. When real rates are high, holding gold has a large opportunity cost (you forgo the real yield you could earn on bonds), so gold tends to fall. When real rates are low or negative, that opportunity cost vanishes and gold tends to rise. Empirically, gold's return has a strong negative relationship with changes in real yields. This makes gold a hedge against a specific scenario, inflation that the central bank is not responding to, so that real rates fall, rather than a hedge against inflation in general.

---

## 4. Inflation hedging - which assets protect purchasing power

Now the core question of the lesson: if you expect inflation you did not previously see coming, what do you hold? Ranking the main candidates by how well they defend real returns.

Nominal government **bonds** are the worst place to be. A bond pays fixed nominal coupons; when inflation rises, those fixed payments are worth less in real terms, and yields rise (prices fall) to compensate new buyers. They have a strongly negative inflation beta and are the classic loser in an inflation shock, as 2022 demonstrated.

**Inflation-linked bonds** (in the US, TIPS, Treasury Inflation-Protected Securities) are made for this. When prices rise, this bond’s principal (face value) rises with the CPI, so its real value is protected automatically. Take a normal bond and an inflation-linked bond of the same maturity: the gap between their yields is the breakeven inflation rate, which is the inflation the market already expects over that period (as we discussed in previous lesson). TIPS pay off when actual inflation comes in above that expected level. If inflation just matches what was expected, you gain nothing extra. You need inflation to surprise on the high side.

**Commodities** have the highest inflation beta and are the most direct hedge against unexpected inflation, because commodity prices are themselves a large component of inflation. When energy and food prices spike, that *is* the inflation. The cost is that commodities are volatile, cyclical, and as we discussed can bleed return through contango even when spot prices hold.

**Gold** hedges a narrower scenario (falling real rates), as discussed. It is a good hedge against monetary debasement and negative-real-rate environments, a weaker hedge against inflation that central banks fight aggressively.

**Equities** are a mixed and often misunderstood case. Over long horizons stocks tend to outpace inflation, because companies can raise prices and their earnings grow in nominal terms. But over the short-to-medium horizon of an inflation shock, equities usually fall: rising inflation brings rising rates, which lowers the present value of future earnings, and squeezes margins. So equities are a long-run inflation hedge but a short-run inflation casualty, a distinction that matters enormously for which horizon you are managing.

Real assets, real estate, infrastructure, REITs (Real Estate Investment Trusts, covered in Lesson 11), generate income linked to replacement costs and rents that often reset with inflation, giving them moderate positive inflation betas, though they also carry interest-rate sensitivity that can offset the hedge.

The summary table:

| Asset | Inflation beta | Hedges against | Main drawback |
|---|---|---|---|
| Nominal govt bonds | Strongly negative | Nothing (deflation/recession, if anything) | Worst asset in an inflation shock |
| Inflation-linked bonds (TIPS) | Positive by construction | Realized inflation above breakeven | Underperforms if inflation only meets expectations |
| Commodities | Strongly positive | Unexpected inflation, energy/food shocks | Volatile; contango drag; cyclical |
| Gold | Positive (via real rates) | Falling real rates, monetary debasement | No yield; weak when real rates rise |
| Equities | Low / slightly negative short-run | Inflation over the long run only | Falls during the inflation shock itself |
| Real estate / REITs | Moderately positive | Rent-linked inflation | Rate-sensitive; offsets the hedge |

---

## 5. Regime changes and the stock–bond correlation

The most important practical lesson from the 2021–2022 inflation surge concerns the correlation between stocks and bonds, the relationship on which the standard 60/40 portfolio (60% equities, 40% bonds) is built.

The case for holding bonds alongside stocks rests on negative correlation: when equities fall in a recession, bonds rise as rates are cut, cushioning the portfolio. This held for roughly two decades (the 2000s and 2010s) because the dominant shocks were growth shocks, and growth shocks push stocks and bonds in opposite directions (bad growth → stocks down, rates cut, bonds up).

But this correlation is not a law of nature, it depends on whether the dominant shock is a growth shock or an inflation shock. In an inflation shock, both stocks and bonds fall together: inflation lifts rates, which hurts bond prices directly and hurts equities through discounting. In 2022 the stock–bond correlation turned positive, the 40% bond allocation failed to cushion anything, and the 60/40 portfolio had one of its worst years in a century. The diversification that had been reliable for twenty years switched off precisely when it was needed.

The general point, and the reason this sits at the center of the lesson: correlations are regime-dependent. A covariance matrix estimated over one regime can be actively misleading in another. Any allocation, risk model, or backtest that assumes a fixed stock–bond correlation is fragile to a change in the inflation regime. This is why practitioners increasingly test allocations separately across inflation regimes rather than over one pooled sample.

---

## 6. Building an inflation-aware allocation

The practical response is to *stop treating any single portfolio as robust across all regimes*, and instead ask what protects the portfolio in each. This is the intuition behind "all-weather" or risk-balanced allocations (Bridgewater's framing): rather than concentrate in assets that do well in the growth-shock, low-inflation regime that happened to dominate recent history, spread exposure so that some part of the portfolio benefits in each macro state, growth up, growth down, inflation up, inflation down. Inflation-up is the state a conventional stock-heavy or 60/40 portfolio is least protected against, which is exactly the gap commodities, linkers, and gold are meant to fill.

The trade-off is that *inflation hedges are a cost in the far more common non-inflationary regime*: commodities are volatile and can bleed through contango, gold yields nothing, and linkers (like TIPS)underperform nominal bonds when inflation stays low. An inflation hedge is a form of insurance, and like insurance it has an ongoing premium. Whether it is worth paying depends on how likely the inflationary regime is and how much the investor would suffer in it.

---

## 7. Where these strategies break

- Short high-inflation samples. There have been very few sustained inflation episodes in the modern data (the 1970s, briefly 2021–22). Every inflation-hedge backtest is therefore estimated on a handful of episodes, and confidence intervals around any inflation beta are wide. Treat precise hedge ratios with suspicion.
- Regime-blind backtests. As in Section 5, a strategy backtested over a pooled sample dominated by the low-inflation regime will look one way in aggregate and behave very differently in the regime you bought it for. Always test performance conditional on the inflation state, not just on average.
- Spot vs total return. Using spot commodity prices instead of a total-return futures index overstates achievable returns, often by a lot (Section 3). This single error invalidates many published commodity results.
- Expected vs unexpected inflation. Regressing returns on the *level* of inflation confuses what was already priced with the surprise. The hedging property lives in the response to the surprise; a naive regression on realized inflation understates it.
- Correlation instability. Any hedge ratio, risk-parity weight, or optimizer output built on a covariance matrix from one regime can be wrong-signed in another. The stock–bond flip is the clearest example, but it is not the only one.
- Gold is not a general inflation hedge. Over short and medium horizons gold tracks real rates more than inflation; in periods where the central bank raised real rates to fight inflation, gold fell *during* the inflation. Selling it as "the inflation hedge" without qualification is wrong.

---

## 8. Reading assignment (for next lesson)

Papers

Park & Irwin (2007), *What Do We Know About the Profitability of Technical Analysis?* [Link](https://onlinelibrary.wiley.com/doi/10.1111/j.1467-6419.2007.00519.x)

Moskowitz, Ooi & Pedersen (2012), *Time Series Momentum* [Link](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2089463)

Lo, Mamaysky & Wang (2000), *Foundations of Technical Analysis* [Link](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=228099)

Primary reading

Chan, Algorithmic Trading: Winning Strategies and Their Rationale (signal construction and backtesting); López de Prado, Advances in Financial Machine Learning (backtest overfitting, used in Section 6).

Supplementary reading 

David Aronson, Evidence-Based Technical Analysis, Chs. 1–2 and the chapter on data-mining bias. A statistical treatment of why most technical rules fail testing.

Perry Kaufman, Trading Systems and Methods, the chapters on moving averages, momentum oscillators, and volatility bands, as a reference for indicator definitions and standard parameters.



---

## 9. Home tasks

All use free data. Deliver short report + reproducible code. Deadline is end of the nearest Wednesday. Report real (inflation-adjusted) returns where relevant, and where a task involves a hedge, always test performance *conditional on the inflation regime*, not just on the full sample.

1. Inflation betas of the major asset classes.
   Assemble monthly total returns for a set of assets: US equities (S&P 500), nominal Treasuries (a bond ETF proxy), a broad commodity total-return index, gold, and a REIT ETF. Regress each asset's return on monthly inflation (CPI change) and report the estimated inflation beta with standard errors. Rank the assets and compare your ranking to Section 4.
   *Stretch (optional):* separate inflation into expected and unexpected components (e.g. use a simple forecast or the breakeven rate as the "expected" part) and re-run the regressions on the unexpected part. Show that the hedging betas are stronger against the surprise.

2. The stock–bond correlation regime flip.
   Compute the rolling correlation (e.g. 24-month window) between monthly S&P 500 returns and monthly 10-year Treasury returns, from 1970 or as far back as data allows. Plot it alongside trailing inflation. Identify the periods where the correlation is positive vs negative and relate them to the inflation regime. Then quantify what the sign flip did to a 60/40 portfolio: compare 60/40 drawdowns in the negative-correlation regime vs the positive-correlation regime.

3. The dollar as a global factor: EM vs DM.
   Take monthly returns of an emerging-market equity index and a developed-market (ex-US or US) equity index, and the US dollar index (DXY) from FRED. Regress EM and DM returns on dollar changes and compare the sensitivities. Then split the sample into strong-dollar and weak-dollar regimes and show how EM's relative performance and its correlation with US equities differ across the two. Comment on what this does to the diversification case for EM.

4. Commodity total-return decomposition and gold vs real rates.
   Core: for one commodity (crude oil is the clearest), obtain a front-month futures or ETF series and a total-return series, and decompose the total return into spot and roll components. Show the contango drag: quantify how much return was lost to rolling over the sample, and relate it to periods of contango vs backwardation.
   Stretch: separately, regress monthly gold returns on changes in the 10-year real yield (TIPS). Show the negative relationship, and identify the periods where it broke down.

5. Backtest: commodities vs equities as an inflation hedge (syllabus core deliverable).
   Build two simple hedge sleeves, a commodity total-return index and an equity index, and test each as an inflation hedge over 20+ years. The key step is conditioning: split all months into "high-inflation" months (e.g. CPI year-on-year above a threshold, or in the top quintile) and "normal" months, and compare each asset's *real* return in the two states. Report, for each asset and each state, the mean real return, volatility, and a hedge quality measure (e.g. its inflation beta within that state). Conclude which asset actually protected purchasing power during inflation, and at what cost during normal times. Discuss the small-sample problem from Section 7 explicitly.
