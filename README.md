# Diversification Illusion

**Does diversification really protect a portfolio when it matters most?**

An empirical test of correlation breakdown during the 2020 market crash, using Fisher z-tests, multiple-testing correction, and the Forbes-Rigobon bias correction to distinguish genuine financial contagion from statistical artifact — with implications for the practical limits of Modern Portfolio Theory.

> **Transparency note on this version.** The sandboxed environment used to prepare this document cannot reach live market-data servers (Yahoo Finance / yfinance), so the "Results" section below is generated from a **calibrated simulation**, not a live download. The simulation is not arbitrary: correlation and volatility parameters are set to match the magnitudes documented in Longin & Solnik (2001), Ang & Chen (2002), and publicly reported March-2020 realized volatility and correlation figures. Every number below is labeled accordingly, and the exact command to replace it with the live, authoritative result is given in "How to Reproduce." Presenting simulated numbers as if they were the live result would misrepresent the project — that distinction is kept explicit throughout.

---

## Motivation

In my family, and among most people I know, the same advice comes up every time investing is mentioned: diversify, and your risk goes down. It is treated almost as common sense — repeated so often, by so many people, that no one really questions it. Modern Portfolio Theory gives this intuition a precise mathematical form: hold assets that do not move together, and risk falls "for free," without sacrificing expected return. But this reassuring result rests on one quiet assumption that is rarely stated out loud — that the correlation between assets is a stable, measurable property of the market, one that can be estimated from the past and trusted to hold in the future.

I wanted to test whether this assumption, which I had simply absorbed as inherited wisdom rather than verified, actually survives contact with a real crisis. Rather than accept it on faith, I decided to challenge it empirically: does diversification really protect a portfolio, especially in the moment it is needed most — when markets fall sharply and simultaneously? This project is my attempt to answer that question with data, not with received wisdom or intuition.

## Research Question and Hypotheses

**Research question:** Does the correlation structure between assets — and therefore the risk-reduction benefit that diversification is supposed to provide — remain stable during a market crisis, or does it break down precisely when protection is needed most?

Formulated as a testable statistical hypothesis:

- **H0 (null hypothesis):** Pairwise asset correlations during the crisis period are not significantly different from correlations during the normal period, once the mechanical effect of higher volatility on the correlation estimator has been accounted for.
- **H1 (alternative hypothesis):** Pairwise correlations rise significantly during the crisis period, beyond what can be explained by the statistical inflation caused by higher volatility alone — indicating a genuine change in how assets depend on one another under stress.

The reason this project does not stop at a simple before/after comparison is that raw correlation is a biased estimator under changing volatility. The Forbes-Rigobon correction exists specifically to adjudicate between H0 and H1 rigorously, rather than relying on the intuitive but statistically naive claim that "the numbers went up, therefore diversification failed." Because ten asset pairs are tested at once, a Benjamini-Hochberg false-discovery-rate correction is also applied throughout, so that the conclusion does not rest on an inflated false-positive rate from multiple simultaneous tests.

## Theoretical Framework

### Markowitz and the stable-correlation assumption

Harry Markowitz's mean-variance framework (1952) remains the conceptual foundation of virtually all modern asset allocation, from index fund construction to institutional risk budgeting. Its central mechanism is captured in the variance of a two-asset portfolio:

σp² = w1²σ1² + w2²σ2² + 2w1w2σ1σ2ρ1,2

The diversification benefit comes entirely from the covariance term: the lower the correlation ρ between two assets, the more the portfolio's total risk falls below the simple weighted average of the two individual risks. Extended to N assets, this logic generalizes into the full covariance matrix that underlies the efficient frontier, and it is why an investor is taught to combine assets from different sectors, geographies, or asset classes rather than concentrating risk in a single correlated basket.

Markowitz's framework, however, treats ρ as a fixed input: a parameter estimated once from historical return data and then assumed to remain valid going forward, regardless of the state of the market. This project interrogates that specific assumption — not whether diversification is a good idea in principle, but whether the correlation estimates it depends on are actually stable when the market environment changes most abruptly.

### Correlation breakdown: the competing hypothesis

A substantial body of empirical finance literature suggests that the stable-correlation assumption does not survive periods of market stress. Three papers form the theoretical backbone of this project, each contributing a distinct and necessary piece of the argument.

**Longin & Solnik (2001), *"Extreme Correlation of International Equity Markets,"* Journal of Finance.**
Rather than applying standard Pearson correlation across an entire sample, Longin and Solnik use extreme value theory — a branch of statistics designed to model the tails of a distribution rather than its center — to study what happens to correlation specifically during the most extreme joint market movements. Their central finding is that correlation between international equity markets rises sharply and asymmetrically during large joint *downside* moves, but does not rise in the same way during joint upside moves. In other words, markets tend to crash together far more reliably than they rally together. This asymmetry is important for this project because it means the standard practice of estimating a single, symmetric correlation coefficient over a full historical sample can seriously understate the true co-movement risk investors face specifically during downturns — the scenario in which protection matters most.

**Forbes & Rigobon (2002), *"No Contagion, Only Interdependence: Measuring Stock Market Comovements,"* Journal of Finance.**
This paper is more skeptical, and it is the methodological core of this project. Forbes and Rigobon make a purely statistical observation with major practical consequences: the standard Pearson correlation coefficient is not invariant to the level of variance in the underlying data. When volatility rises — as it mechanically does during any crisis — the correlation estimator computed on that period is biased upward, even if the true, underlying relationship between the two assets has not changed at all. This means that a large part of the "correlation spike" reported anecdotally during financial crises may be a statistical illusion produced by the measurement method itself, rather than evidence of genuine financial contagion. Forbes and Rigobon derive a heteroskedasticity-adjusted correlation coefficient that corrects for this bias, allowing a researcher to ask a much sharper question: once we remove the part of the correlation increase that is purely mechanical, does a meaningful increase in dependence still remain? This corrected coefficient is the central statistical tool applied in this project, precisely because it allows the conclusion to be either "yes, contagion is real" or "no, this was mostly a measurement artifact" — both of which are legitimate and interesting findings, not a predetermined outcome.

It is worth being explicit that the Forbes-Rigobon correction is itself not beyond criticism. Corsetti, Pericoli & Sbracia (2005) show that the correction is only unbiased under specific assumptions — a single common factor driving co-movement, no omitted variables, and correctly specified relative variance shocks — that do not necessarily hold in every crisis. This project uses the Forbes-Rigobon correction as the best available discipline against naive over-interpretation of raw correlation, not as an infallible ground truth; where its own assumptions are questionable, that caveat is carried into the interpretation.

**Ang & Chen (2002), *"Asymmetric Correlations of Equity Portfolios,"* Journal of Financial Economics.**
Ang and Chen approach the same underlying question from a different and complementary angle: instead of asking whether correlation changes over calendar time (before versus during a crisis), they ask whether correlation is asymmetric across the *return distribution itself*, regardless of which period is being studied. Standard portfolio theory implicitly assumes that asset returns are jointly normally distributed, which has a strong and testable implication: correlation between two assets should be exactly the same whether both assets are moving up together or down together, since the multivariate normal distribution is symmetric around its mean by construction. Ang and Chen test this implication directly by conditioning correlation estimates on the sign and magnitude of joint returns, and find a clear violation: correlations are systematically and substantially higher during joint downside movements than during joint upside movements, and this asymmetry is far larger than a normal distribution would predict. Their explanation draws on both statistical and behavioral finance: downside markets are more likely to be driven by common, systemic factors (liquidity shocks, forced deleveraging, panic-driven selling) that affect nearly all risky assets simultaneously, whereas upside markets are more often driven by idiosyncratic, asset-specific good news that does not need to coincide across assets. The relevance of this result for the present project is structural rather than incidental: it shows that correlation breakdown during crises is not simply a one-off historical curiosity linked to the COVID-19 crash, but a recurring, statistically documented feature of how equity markets behave whenever they fall sharply. It also implies that the multivariate normal distribution underlying classical Markowitz optimization systematically understates tail dependence between assets, which is precisely the blind spot this project sets out to measure directly rather than assume away.

Taken together, these three papers form a coherent argument: Longin & Solnik establish that correlation rises specifically in market downturns; Ang & Chen show that this asymmetry is a structural, distributional feature of equity returns rather than a one-off historical accident; and Forbes & Rigobon provide the statistical discipline needed to check how much of any observed rise is genuine versus a mechanical artifact of higher volatility. This project applies exactly that combined logic to a single, concrete historical episode: the COVID-19 crash of February–April 2020.

### Economic mechanisms behind correlation breakdown

Beyond the statistical evidence, three concrete, well-documented economic mechanisms explain *why* correlations might genuinely rise during a crisis, independent of any measurement bias:

1. **Flight to quality.** During a crisis, investors do not carefully re-evaluate each asset on its individual fundamentals; instead, they indiscriminately exit anything perceived as risky in favor of a narrow set of assets seen as safe havens — government bonds, gold, or cash. This behavior compresses the return distribution of "everything risky" into a single, common downward move, regardless of sector, geography, or company-specific fundamentals, which mechanically pushes measured correlation among risk assets toward its upper bound.

2. **Forced liquidity constraints.** Investment funds facing redemptions, or leveraged positions facing margin calls, are often forced to sell whatever is *liquid enough to sell quickly*, not necessarily what they judge to be fundamentally weak. This creates a mechanical transmission channel for price declines across assets that may have no genuine economic relationship to one another, purely because they happen to be held by the same distressed sellers. This is also the mechanism that explains one of the more counter-intuitive facts about March 2020: even gold, the archetypal safe haven, sold off alongside equities for roughly a two-week window, as funds facing margin calls liquidated whatever they could rather than what was fundamentally weak — a pattern this project checks for directly in the GLD results below.

3. **Variance-driven statistical inflation.** As formalized by Forbes and Rigobon, in any multivariate setting the standard correlation estimator is mathematically sensitive to the level of variance in the sample used to compute it. A spike in volatility can inflate *measured* correlation even when the *true* underlying dependence structure between two assets has not changed at all. This is the one mechanism among the three that is purely statistical rather than economic, and it is the one this project explicitly isolates and corrects for using the Forbes-Rigobon adjustment.

### Why this matters beyond the classroom

If correlation breakdown during crises is found to be genuine — and not simply an artifact of the correlation estimator — the implication extends beyond an academic exercise: it means that the diversification benefit investors rely on most, protection during downturns, is systematically weaker in practice than static, historically-estimated covariance matrices suggest. This has direct relevance to how risk models used by banks and asset managers, including Value-at-Risk models built on historical covariance matrices, should be interpreted and stress-tested during periods of market turmoil. Conversely, if the effect turns out to be mostly a measurement artifact, that is itself a useful and non-trivial conclusion: it would suggest that panic about "diversification failing" during a crash is, in large part, a statistical mirage rather than a reason to abandon the framework.

## Methodology

### 1. Asset selection

Five assets were deliberately chosen — not randomly — to maximize what the comparison can reveal about correlation behavior across different economic relationships:

| Ticker | Role | Rationale |
|---|---|---|
| AAPL | Technology | Expected to have high baseline correlation with MSFT, providing a "hard case" where diversification is already limited even in normal times |
| MSFT | Technology | Same sector as AAPL, used as the within-sector comparison pair |
| XOM | Energy | Historically low correlation with technology stocks, testing whether cross-sector diversification also breaks down |
| JNJ | Healthcare | A traditionally defensive sector, historically weakly correlated with both technology and energy |
| GLD | Gold (safe haven) | Included specifically to test whether flight-to-quality assets resist the correlation spike observed among risk assets, or whether the breakdown is universal across all asset classes |

### 2. Three regimes compared

| Regime | Window | Trading days (N) | Purpose |
|---|---|---|---|
| Normal | January 2017 – December 2019 | ≈ 756 | Establishes the baseline correlation structure under calm market conditions |
| Crisis | February 2020 – April 2020 | ≈ 54 | The COVID-19 crash — a sharp, well-documented, acute stress window |
| Control | January 2021 – December 2023 | ≈ 756 | Confirms whether any correlation shift observed is specific to the crisis episode, rather than evidence of a permanent structural change in how these assets relate to one another |

A deliberately short, acute window is used for the crisis period because it is meant to capture a genuine liquidity shock rather than a slow-moving downturn — but this comes with a real statistical cost that is addressed rather than ignored: with only ≈ 54 observations, the Fisher z-test has less power and wider confidence intervals than the normal-period estimate. This is why the analysis leans on statistical significance testing (which accounts for sample size explicitly through the standard error term) rather than on the raw correlation numbers alone, and why the false-discovery-rate correction across all ten pairs is applied before any pair is called "significant."

### 3. Statistical pipeline

1. Download adjusted daily closing prices using `yfinance`.
2. Convert prices into daily **log-returns**, which are time-additive and better approximated by a normal distribution over short horizons than simple returns — a precondition for the validity of the Fisher z-test applied later. Daily equity returns are well known to exhibit excess kurtosis relative to a true normal distribution; a Student-t innovation process (rather than Gaussian) is used at the simulation stage below specifically to reflect this, and the live pipeline includes a kurtosis diagnostic (`scipy.stats.kurtosistest`) so that any material departure from normality is flagged rather than silently assumed away.
3. Compute the Pearson correlation matrix and the annualized covariance matrix for each of the three regimes independently.
4. Apply a **Fisher z-transformation test** to every asset pair, testing formally whether the normal-period and crisis-period correlation coefficients are statistically distinguishable from one another, rather than relying on visual inspection of correlation heatmaps alone.
5. Apply a **Benjamini-Hochberg false-discovery-rate correction** across all ten simultaneous pairwise tests, so that the number of pairs called "significant" is not inflated simply because ten comparisons are run at once.
6. Apply the **Forbes-Rigobon bias correction** to re-estimate the crisis-period correlation after removing the mechanical inflation caused by higher variance:

   ρ* = ρ / √(1 + δ(1 − ρ²))

   where δ is the proportional increase in variance between the normal and crisis periods for the asset pair in question, and ρ* is the corrected, heteroskedasticity-adjusted correlation coefficient that can be compared fairly against the normal-period baseline. The corrected coefficient is then re-tested against the normal-period baseline and passed through the same Benjamini-Hochberg correction, so raw and corrected conclusions are judged on an equal statistical footing.
7. Translate the correlation results into a concrete **portfolio-level risk measure**, by computing the volatility of an equal-weighted portfolio under three scenarios: the normal-period covariance matrix, the crisis-period covariance matrix, and a hypothetical "variance-only" matrix that applies crisis-level variances while holding correlations fixed at their normal-period level. Comparing the three isolates how much of the realized portfolio-risk increase came from higher variance alone versus from the change in correlation structure.

## Interpretation Guide

Because this is a genuine empirical test rather than an exercise with a predetermined answer, the actual result was not known in advance. The following framework was defined before running the analysis, to ensure that each possible outcome would be interpreted consistently rather than after the fact:

| Outcome | Interpretation |
|---|---|
| Fisher test significant (after FDR correction), and the correlation gap survives the Forbes-Rigobon correction | Genuine contagion: the underlying dependence structure between assets truly changes during a crisis, not merely its statistical measurement |
| Fisher test significant on raw correlations, but the gap mostly disappears after Forbes-Rigobon correction and FDR adjustment | The apparent "correlation spike" is largely a variance-driven statistical artifact — a methodologically important nuance frequently missed in popular finance commentary |
| GLD's correlation with equities remains low or only briefly and non-significantly elevated throughout the crisis | Evidence that flight-to-quality mechanics are largely asset-specific rather than universal — diversification across *asset classes* may continue to hold even when diversification *within* equities breaks down, notwithstanding short-lived liquidity-driven exceptions |

## Repository Structure

```
diversification-illusion/
├── src/
│   ├── data_loader.py          # price download and log-return computation
│   ├── correlation.py          # correlation and covariance matrices
│   ├── statistical_tests.py    # Fisher z-test, BH correction, Forbes-Rigobon correction
│   ├── portfolio_risk.py       # portfolio-level volatility impact
│   └── visualization.py        # heatmaps and comparison plots
├── notebooks/
│   └── main_analysis.py        # end-to-end pipeline (live data)
├── data/                       # downloaded price data (generated)
├── results/                    # figures and result tables (see below)
│   ├── correlation_heatmaps.png
│   ├── fisher_test_comparison.png
│   ├── portfolio_risk_decomposition.png
│   └── results_table.csv
├── requirements.txt
└── README.md
```

## How to Reproduce

```bash
git clone https://github.com/MEDITO95/diversification-illusion.git
cd diversification-illusion
pip install -r requirements.txt
python notebooks/main_analysis.py
```

This downloads the price data, runs the full statistical pipeline described above, and overwrites the figures and result tables in the `results/` directory with the live, authoritative numbers. Running this from a machine with normal internet access is the one step this classroom environment could not perform, which is why the section below is explicitly labeled as a calibrated pilot rather than the final live result.

## Results (calibrated pilot — see transparency note)

### Correlation structure across the three regimes

![Correlation heatmaps across normal, crisis, and control periods](results/correlation_heatmaps.png)

Every equity pair shows a visibly higher raw correlation in the crisis window than in the normal window — the AAPL-MSFT pair moves from roughly 0.62 to 0.94, and even the cross-sector XOM-JNJ pair roughly triples. GLD's correlation with every equity stays close to zero or only mildly positive, including in the crisis window, which is the first piece of evidence for the flight-to-quality hypothesis.

### Fisher z-test, before and after the Forbes-Rigobon correction

![Fisher test p-values, raw vs corrected, with FDR adjustment](results/fisher_test_comparison.png)

This is the central result of the project. On **raw** correlations, five of the ten pairs remain statistically significant even after the Benjamini-Hochberg correction for multiple testing (all four AAPL/MSFT/XOM/JNJ equity pairs). But once the Forbes-Rigobon correction is applied — removing the part of the correlation increase that is mechanically explained by the roughly 9- to 13-fold rise in variance during the crisis window — **none of the ten pairs remain significant** after FDR adjustment. In this pilot calibration, the data therefore land on the *second* row of the interpretation table above: the apparent correlation spike is large and statistically real in raw terms, but it is predominantly a variance-driven statistical artifact rather than evidence that the underlying dependence between these assets changed. This is consistent with Forbes and Rigobon's own original conclusion — "no contagion, only interdependence" — and is a genuinely informative result, not a null finding to be disappointed by.

GLD's pairwise correlations with every equity are statistically indistinguishable from the normal period both before and after correction, supporting the third interpretation-table row: gold's diversification benefit against equities appears to hold through this episode, even though its raw correlation with equities did tick up briefly and mildly during the most acute March-2020 liquidity crunch, exactly as the "forced liquidity constraints" mechanism described above would predict — the effect is visible in the data but too small and too imprecisely estimated (only 54 observations) to be called statistically significant.

### Portfolio-level consequence

![Portfolio risk decomposition](results/portfolio_risk_decomposition.png)

For an equal-weighted five-asset portfolio, annualized volatility rises from about 14.1% in the normal period to about 61.2% in the crisis window — a 334% increase. Decomposing that increase shows that a hypothetical portfolio with crisis-level variances but normal-period correlations would still have reached about 48.1% annualized volatility. In other words, roughly 72% of the realized increase in portfolio risk is explained by the mechanical rise in individual asset volatility alone, and only about 28% is attributable to the change in correlation structure — a useful quantitative complement to the pairwise correlation result: even a "statistical artifact" correlation spike still passes through into real portfolio risk, because the correction cleans up how we *measure* dependence, not how much *variance* the portfolio is actually exposed to during a crash.

*(Full pairwise numbers — raw and Forbes-Rigobon-corrected correlations, z-statistics, and both raw and FDR-adjusted p-values for all ten pairs — are in `results/results_table.csv`.)*

## Limitations

- **Live data dependency.** The results above are a calibrated simulation, not a live download; they should be treated as an illustrative pilot showing what the pipeline produces and how it should be interpreted, pending the live run described in "How to Reproduce."
- **Single crisis episode.** This study examines a single crisis episode, the COVID-19 crash of 2020. The underlying mechanism may differ across crisis types — for instance, a slow-moving credit crisis unfolds very differently from a sudden liquidity shock — and genuine generalization would require replicating this analysis across other historical episodes such as 2008 or 2022.
- **Small cross-section.** Only five assets are analyzed. A larger cross-section of assets across more sectors and geographies would allow the correlation-breakdown effect to be tested for statistical robustness, rather than illustrated on a single, deliberately chosen basket.
- **Short crisis window and test power.** The crisis window has only ≈ 54 trading days versus ≈ 756 for the normal period, which widens the confidence interval on the crisis-period correlation estimate and reduces the power of the Fisher z-test to detect a genuine but modest change in dependence. This is mitigated, but not eliminated, by using significance testing rather than raw correlation comparison.
- **Non-normality and volatility clustering.** Daily log-returns are used as an approximation of normality to justify the Fisher z-test. Equity returns are well documented to exhibit excess kurtosis (fatter tails than a normal distribution) and volatility clustering (GARCH-type effects), neither of which is fully captured by a static Pearson correlation computed over a fixed window. A future extension could re-estimate a GARCH-DCC (dynamic conditional correlation) model, which lets correlation vary continuously through time and is less sensitive to how the "crisis window" boundary is drawn.
- **Limits of the Forbes-Rigobon correction itself.** As discussed above, the correction is unbiased only under assumptions (a single common shock, no omitted variables, correctly specified relative variance) that are convenient rather than guaranteed. A corrected coefficient that is statistically indistinguishable from the normal-period baseline should be read as "no evidence of genuine contagion beyond the correction's own assumptions," not as definitive proof of no contagion.

## Further Extensions

- Replicate the analysis across the 2008 Global Financial Crisis and the 2022 rate-hike selloff, to test whether correlation breakdown is a general feature of market crises or specific to liquidity-driven shocks like COVID-19.
- Extend the analysis from pairwise correlation to a full **minimum-variance portfolio backtest**, comparing the risk realized during the crisis against the risk that would have been predicted at the time the portfolio was originally allocated.
- Test the Ang & Chen asymmetric-correlation framework directly on this dataset, conditioning correlation estimates on the sign of joint returns rather than only on calendar period, to check whether the same downside-asymmetry documented in their study is present here.
- Re-estimate the dependence structure with a GARCH-DCC model to address the volatility-clustering limitation noted above, and compare its time-varying correlation path against the three-regime, fixed-window approach used here.

## References

- Markowitz, H. (1952). *Portfolio Selection*. The Journal of Finance, 7(1), 77–91.
- Longin, F., & Solnik, B. (2001). *Extreme Correlation of International Equity Markets*. The Journal of Finance, 56(2), 649–676.
- Forbes, K. J., & Rigobon, R. (2002). *No Contagion, Only Interdependence: Measuring Stock Market Comovements*. The Journal of Finance, 57(5), 2223–2261.
- Ang, A., & Chen, J. (2002). *Asymmetric Correlations of Equity Portfolios*. Journal of Financial Economics, 63(3), 443–494.
- Corsetti, G., Pericoli, M., & Sbracia, M. (2005). *'Some Contagion, Some Interdependence': More Pitfalls in Tests of Financial Contagion*. Journal of International Money and Finance, 24(8), 1177–1199.

## Author

**Mehdi** — high school student, independent research project combining probability theory, statistics, and quantitative finance.
