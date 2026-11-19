# Lesson 7 — Behavioural and Event-Driven Strategies

This lecture covers two related families of strategies. The first, **event-driven**, takes positions tied to specific corporate events (mergers, bankruptcies, spin-offs) whose outcome is largely **independent of the overall market direction**. The second, behavioural, exploits systematic errors in **how investors react to information**, and includes strategies built from news and social-media sentiment. The two are grouped together because they share the same underlying justification. Both earn returns because prices do not adjust to information instantly and rationally, either because the event outcome carries a risk that must be compensated (event-driven), or because investors under- or over-react to news (behavioural). The second half of the lecture is concrete about how a sentiment signal is built from text and how such a signal is tested honestly.


Primary reading: Lhabitant, *Handbook of Hedge Funds* (the event-driven chapters: merger arbitrage, distressed, special situations); Damodaran, *Investment Valuation* (the distressed-firm valuation chapter, for Section 4); López de Prado, *Advances in Financial Machine Learning* (treating text as features, and backtest overfitting, for Sections 7 and 8).

Supplementary reading:
- Shleifer, *Inefficient Markets: An Introduction to Behavioural Finance* — the chapters on limits to arbitrage, as the foundation for Section 1.
- Tetlock (2007), "Giving Content to Investor Sentiment: The Role of Media in the Stock Market" (*Journal of Finance*) — the reference for Section 6; established that news tone predicts returns.
- Loughran and McDonald (2011), "When Is a Liability Not a Liability? Textual Analysis, Dictionaries, and 10-Ks" (*Journal of Finance*) — the finance-specific sentiment dictionary, for Section 7.
- Mitchell and Pulvino (2001), "Characteristics of Risk and Return in Risk Arbitrage" (*Journal of Finance*) — the payoff profile of merger arbitrage, for Section 3.

---

## 1. Base of these strategies

For a **mispricing** to persist, two conditions must hold: some investors must misprice an asset, and other investors must be unable or unwilling to correct it. Behavioural finance supplies the first condition, and the theory of limits to arbitrage supplies the second.

On the **mispricing side**, investors are observed to react to information in predictable, non-instant ways. Two patterns matter for this lecture.

**Underreaction** - prices move too little on the day new information arrives, and continue drifting in the same direction for days or weeks afterward. This is the basis for sentiment signals (Section 6) and for post-earnings drift (Section 5), and it is the same continuation effect studied as momentum in Lesson 6.

**Overreaction** - prices move too far on strong news or sentiment, then reverse. This is the basis for the short-horizon reversal studied in Lesson 6, and it explains why some sentiment signals predict a reversal rather than a continuation.

On the *correction side*, an trader who spots the mispricing may still not trade it away, for reasons Shleifer sets out. The mispricing can widen before it closes, forcing a leveraged trader out of the position at a loss. The hedge may be imperfect. The cost of shorting may be high. The event that resolves the mispricing may be far off. These frictions are why the mispricing survives long enough to be a strategy, and they are also why these strategies carry real risk rather than being free money.

We can assume that for each strategy, **the mispricing comes from a behavioural error, and the return comes from bearing the risk** that the error takes time and uncertainty to resolve.

---

## 2. Event-driven strategies: the common structure

An event-driven strategy takes a position whose **payoff depends on the outcome of a specific corporate event** rather than on the market. The events are things like: a merger, a bankruptcy filing, a spin-off, an index reconstitution, or a proxy fight. Because the payoff is tied to the event, the return is largely uncorrelated with the market index, which is why these strategies are valued in a portfolio

The return has two parts. One part is compensation for **bearing event risk**, the chance that the event does not resolve as expected (a deal breaks, a reorganisation fails). This part is a risk premium and is present even when the manager has no special skill. The **other part comes from analysis, estimating the probability and timing** of the outcome better than the market. Distinguishing these two parts matters for the same reason as in Lesson 1: only the second justifies a performance fee, and only the first is straightforward to replicate.

A recurring feature of these strategies is an asymmetric payoff. The position typically earns a small, fairly steady gain when the event resolves as expected, and takes a large loss when it does not. This shape resembles selling insurance, and it means the reported volatility understates the true risk, because the large losses are rare and may not appear in a short sample. Section 3 shows this concretely for merger arbitrage.

---

## 3. Merger (risk) arbitrage

When company A announces it will acquire company T, the target's share price jumps toward the offer price but usually stays below it (see recent Delivery Hero stock prices). The **gap is the deal spread. It exists because the deal might not close** (regulators block it, financing falls through, shareholders vote no), and because the money is tied up until closing.

The basic trade captures this spread. The mechanics depend on the form of the offer.

In a **cash deal**, the acquirer offers a fixed price per target share. The arbitrageur buys the target and holds to closing, earning the spread if the deal completes.

In a **stock deal**, the acquirer offers its own shares at a fixed exchange ratio. The arbitrageur buys the target and short-sells the acquirer in the ratio of the offer, so that the market exposure of the two legs cancels and only the spread remains.

The **return, if the deal closes, is the spread relative to the price paid,** converted to an annual rate over the expected time to close:

    annualised return ≈ (offer price − target price) / target price × (365 / days to close)

The **risk is the deal break**. If the deal collapses, the target price falls back toward its pre-announcement level, a loss much larger than the spread that was on offer. Mitchell and Pulvino show that across many deals this produces a payoff that behaves like a written put on the market: modest positive returns most of the time, and large losses concentrated in market downturns, when deals are most likely to fail. The strategy therefore looks calm and steady until it is not, and its Sharpe ratio overstates how safe it is.

---

## 4. Distressed and bankruptcy strategies

Distressed strategies trade the securities of firms that are in or near bankruptcy. These strategies require understanding the capital structure, the ordered set of claims on a firm's assets, and the legal process that resolves them.

When a firm cannot pay its debts, its claims are paid in order of priority. Secured debt is paid first, then senior unsecured debt, then subordinated debt, then preferred equity, and common equity last. In a bankruptcy, the value of the firm is usually not enough to pay everyone in full, so the claims are paid from the top down until the money runs out. The security at the point where the money runs out is called the fulcrum security. It is the one that is partly paid and typically converts into the equity of the reorganised firm, which makes its value the most sensitive to the outcome and the usual focus of the trade.

In the United States, a firm in trouble can file under Chapter 11, which reorganises the firm as a going concern and renegotiates its debts, or Chapter 7, which liquidates it. Distressed investors form a view on which path the firm takes and on the recovery, the fraction of face value each class of claim ultimately receives. The strategies range from passively buying claims below their expected recovery, to loan-to-own, where the investor buys the fulcrum debt specifically to take control of the reorganised company.

**Valuing a distressed firm uses the tools from Lesson 2** with adjustments for the high probability of default and the changing capital structure, which is the reason Damodaran's distressed chapter is on the reading list. A simpler tool, useful for screening rather than valuation, is a statistical bankruptcy-prediction model. The best-known is Altman's Z-score, which combines five accounting ratios into a single number:

    Z = 1.2·X1 + 1.4·X2 + 3.3·X3 + 0.6·X4 + 1.0·X5

where X1 is ***Liquidity**, working capital over total assets, X2 is **Cumulative Profitability**, retained earnings over total assets, X3 is **Operating Efficiency**, EBIT over total assets, X4 is **Market Leverage**, market value of equity over total liabilities, and X5 is **Asset Turnover** sales over total assets. A low Z (below roughly 1.8 in the original manufacturing sample) flags a high probability of bankruptcy within two years. The model is dated and sample-specific, but it is a compact illustration of turning financial statements into a distress signal, and it is the basis for home task 3.

---

## 5. Special situations and other event-driven trades

Beyond mergers and bankruptcies, several smaller event types share the same structure of a corporate action creating a temporary, market-independent mispricing.

**Spin-offs**: when a firm separates a division into a standalone listed company, the new shares are often sold indiscriminately by index funds and others who did not choose to hold them, which can depress the price temporarily.

**Index reconstitution**: when a stock is added to or removed from a major index, index-tracking funds must trade it, and the predictable flow moves the price around the effective date.

**Buybacks and special dividends**: a firm returning cash signals management's view and changes the share count, and the announcement often precedes a drift.

**Activism**: an investor takes a stake and pushes for a change (a sale, a spin-off, a board seat). The announcement of the stake itself often moves the price, and the eventual change is the payoff.

**Post-earnings-announcement drift** (PEAD): after a firm reports an earnings surprise, its price continues to move in the direction of the surprise for weeks. This is an event (the earnings report) that produces the underreaction pattern of Section 1, and it is the direct link between this lecture and the momentum effect of Lesson 6. It is the basis for home task 5.

Worth mentioning are also **disruption funds**, which invest thematically in firms expected to displace incumbents (in areas such as automation or genomics). These are a directional equity bet on a theme rather than an event-driven strategy in the sense above, and they carry high factor and valuation risk. They are noted here for completeness and are closer in kind to the directional equity strategies of Lesson 2.

---

## 6. Sentiment and news analysis

A sentiment signal **turns unstructured text** (news articles, headlines, company filings, analyst notes, social-media posts) **into a number that measures how positive or negative** the coverage of an asset is, and uses that number to predict returns. It rests on the underreaction pattern of Section 1: **if prices adjust to news with a delay, then a measure of the news, available slightly before the price has fully adjusted, has predictive content**.

Tetlock's study of a daily *Wall Street Journal* column is the standard early result. It found that **unusually negative media tone predicted lower prices in the short term followed by a reversal, and that high tone predicted trading volume**. Two lessons carry forward from it. First, the sign of the effect depends on the horizon: at **very short horizons negative sentiment can precede a decline (underreaction), while an extreme sentiment reading can precede a reversal (overreaction)**. The strategy has to commit to a horizon and test which effect dominates there. Second, the **effect is small per article and decays quickly**, so the signal is only useful in aggregate and at reasonably low cost.

Sentiment can be measured at two levels. A **single-asset** signal scores the text about one company and predicts that company's return. A **market-wide** signal (as in Baker and Wurgler's investor-sentiment index) aggregates many sources into one number that predicts which segments of the whole market do well, typically finding that speculative, hard-to-value stocks are the most affected when sentiment is high.

---

## 7. NLP models for scoring text

Turning text into a sentiment number is a **natural-language-processing** (NLP) task. Three approaches, in increasing order of sophistication and cost.

**Dictionary** (lexicon) methods count words from predefined positive and negative lists. A simple score is:

    sentiment = (positive words − negative words) / total words

The method is transparent and cheap, but the choice of dictionary is critical. A general-purpose sentiment dictionary built for ordinary English mislabels financial text, because words such as "liability", "tax", "cost", "capital", and "depreciation" are counted as negative when they are neutral accounting terms. Loughran and McDonald built word lists specifically from financial filings to fix this, and their dictionary is the standard baseline for finance. The practical point for students: the domain of the dictionary must match the domain of the text.

**Machine-learning classifiers** are trained on examples of text already **labelled** positive or negative, and learn which combinations of words predict the label. They handle context better than word counts but need labelled training data and can overfit.

**Transformer models** (for example FinBERT, a version of the BERT language model fine-tuned on financial text) read whole sentences and capture word **order and context**, so they handle negation and phrasing that word counts miss. They are the most accurate and the most expensive to run, and they are effectively black boxes, which makes their errors harder to diagnose.

Whatever the method, the same text problems recur: **negation, sarcasm and irony, and the difference between a fact and a forecast**. A sentiment score is always an approximation, and its errors are correlated across articles about the same event, which reduces its effective information content.

---

## 8. Constructing a sentiment indicator

The pipeline:

**Collect the text**. Gather documents about the asset, each with a timestamp. The timestamp is the single most important field, because it determines when the information could have been traded on.

**Score each document**. Apply one of the methods from Section 7 to get a sentiment number per document.

**Aggregate to the asset-day**. Combine all documents for an asset on a given period into one score, for example the average sentiment, sometimes weighted by article length or source. Record also the volume of coverage, which is itself informative.

**Normalise**. Convert the raw score to a standardised value so it is comparable over time and across assets, typically a z-score against a trailing window, as in the indicator construction of Lesson 5:

    z_t = (sentiment_t − mean of trailing sentiment) / (standard deviation of trailing sentiment)

**Form and test the signal**. Map the normalised score to a position (for example, long when sentiment is high, or the reverse if the tested effect is a reversal), and backtest it with the discipline from Lesson 6.


## Backtesting

The backtest of a sentiment signal has failure modes beyond the usual ones, and these are the point of the exercise.

**Look-ahead through timestamps**. The signal must use only text published before the moment the position is taken, allowing for the lag between publication and when a trade could actually be placed. Using the day's news to trade at that day's open or prior close is a common and fatal error.

**Staleness and novelty**. Repeated coverage of the same event is not new information. A high sentiment score driven by many articles rewriting one story should not be read as a strong fresh signal, and some construction is needed to down-weight repetition.

**Point-in-time text and identifiers**. Headlines get revised, and ticker symbols are reused across companies over time, so historical text must be matched to the correct company as it was then.

**Decay and crowding**. Sentiment signals are widely used, so any edge decays as more capital trades it, and a signal that worked in an old sample may be arbitraged away in a recent one. Testing on a recent hold-out sample, as in Lesson 6, is the check.

---

## Strategies used in practice

The constructions above appear in the industry under standard names. This list is a reference for which strategies these ideas correspond to and where they are run.

Event-driven:
- Merger (risk) arbitrage: capture the deal spread on announced mergers, hedged so the market exposure cancels (Section 3). A capacity-constrained strategy with an insurance-like payoff.
- Distressed and restructuring: trade the fulcrum security through a bankruptcy or reorganisation, sometimes to take control of the firm (Section 4). Long horizon and illiquid.
- Special situations: spin-offs, index changes, buybacks, and other corporate actions (Section 5).
- Activist: take a stake and force a corporate change; overlaps with special situations.
- Credit / capital-structure arbitrage: trade mispricings between a firm's debt and equity, or across its debt tranches.

Behavioural and sentiment:
- News-sentiment signals: score news and filings to predict short-horizon returns (Sections 6 to 8). Run by systematic equity funds as one signal among many.
- Social-media and alternative-data signals: the same idea applied to social posts, web traffic, or satellite data; higher noise and higher crowding.
- Post-earnings-announcement-drift and revision strategies: trade the drift after earnings surprises and analyst-forecast revisions (Section 5). Overlaps with the momentum factor of Lesson 6.

---

## Empirical evidence

Merger arbitrage delivers steady returns with occasional sharp losses, consistent with the written-put payoff of Mitchell and Pulvino; its returns are correlated with the market only in downturns, which is when its diversification is least useful. Distressed strategies earn an illiquidity premium and require long holding periods, and their returns are hard to measure because the positions are not marked to a liquid market. News sentiment has a documented but small and short-lived predictive effect (Tetlock and the large literature after it), useful only in aggregate and highly sensitive to transaction costs and to the look-ahead errors of Section 8. Across all of these, reported volatility understates risk because the losses are rare, skewed, and clustered in bad times, which is the recurring warning of this course and the reason the performance metrics of are needed to judge them.

## Reding (next lesson)

Papers

Michankow, Sakowski, Slepaczuk (2022), *LSTM in Algorithmic Investment Strategies on BTC and S&P500 Index*, [Link](https://www.mdpi.com/1424-8220/22/3/917) - section on performance metrics

Primary reading

Lhabitant, *Handbook of Hedge Funds* (the chapters on risk measurement and performance measurement); Chan, *Algorithmic Trading: Winning Strategies and Their Rationale* (Sharpe ratio, drawdown, and the practical calculation of these metrics); López de Prado, *Advances in Financial Machine Learning* (the reliability of the Sharpe ratio under multiple testing, used in Section 8).
 
Supplementary reading 

- Bacon, *Practical Portfolio Performance Measurement and Attribution* — the reference for the definitions and annualisation conventions of the ratios in Sections 5 and 6.
- Lo (2002), "The Statistics of Sharpe Ratios" (*Financial Analysts Journal*) — the standard-error formula for the Sharpe ratio used in Section 8; short and readable.
- Bailey and López de Prado (2014), "The Deflated Sharpe Ratio" (*Journal of Portfolio Management*) — the correction for multiple testing in Section 8.
- Ilmanen, *Expected Returns* (a syllabus book) — the chapters on risk and on the historical drawdowns of major strategies, for the interpretation in Section 6.

---

## Home tasks

Use free data (news APIs or archives, Kaggle news datasets, price data from yfinance or Stooq, financials from company filings). Report results with the metrics where relevant.

1. News-sentiment signal on one stock.
   Collect at least a year of headlines for one large-cap stock, score each with the Loughran-McDonald word lists, aggregate to a daily sentiment score, and test whether the score predicts the next day's return. Report the result and be explicit about the publication timestamp you used and the lag you assumed before trading.
   Stretch: rescore the same headlines with a pretrained financial transformer (FinBERT), compare the two signals, and estimate the signal's half-life by measuring how the predictive power falls as the holding horizon lengthens.

2. Merger-arbitrage spread.
   Take three to five completed cash mergers from recent years. For each, compute the deal spread at announcement and its annualised return using the formula in Section 3, then compare that to the return actually realised to closing. Report the average and note the time to close.
   Stretch: build a small portfolio that holds all the deals at once, insert one deal that broke (find a real cancelled deal and use its price path), and report the portfolio drawdown to show how one break offsets many completed deals.

3. Bankruptcy prediction with the Altman Z-score.
   Compute the Z-score (Section 4) for a sample of several firms using one year of financial statements, including several firms that filed for bankruptcy within the following two years. Report how well the score separates the two groups.
   Stretch: replace the fixed Altman weights with a logistic regression fitted on part of the sample, evaluate it on the held-out part, and compare its classification to the original fixed-weight score.

4. Social-media sentiment around an event.
   Pick a stock that had a sentiment-driven episode (an earnings surprise or a period of heavy retail attention). Using a social-media or news-volume dataset, build a daily sentiment or attention series and relate it to the stock's returns and realised volatility over the episode.
   Stretch: separate the contemporaneous relationship (sentiment and return moving together on the same day) from any predictive content (sentiment on one day predicting the next day's return), and test whether extreme sentiment precedes a reversal, linking the result to the overreaction pattern of Section 1.

5. Post-earnings-announcement drift.
   For a set of 20 to 30 stocks, define an earnings-surprise measure (reported earnings minus the prior consensus, scaled), and after each report hold the stock in the direction of its surprise for several weeks. Report the average drift and compare it across the largest positive and negative surprises.
   Stretch: condition the drift on the sentiment of the earnings report text (score the release with the method of task 1), and test whether the drift is stronger when the numerical surprise and the text sentiment agree. Relate the result to the underreaction interpretation of momentum in Lesson 6.
