# The Diversification Illusion
## Breakdown of the Dependence Structure Among Risky Assets Under Systemic Stress: An Econometric Approach to Financial Contagion During the COVID-19 Shock (February–April 2020)

**Author:** Tarik — independent research project in quantitative finance
**Status:** protocol finalised; empirical execution pending

---

## Abstract

Modern Portfolio Theory (Markowitz, 1952) rests on a premise that is rarely interrogated: that the correlation matrix between assets constitutes a stationary parameter, estimable once and thereafter invariant to the prevailing state of the market. This project subjects that premise to a rigorous empirical trial, exploiting the COVID-19 shock of February–April 2020 as a natural quasi-experiment. By jointly deploying the Fisher z-transformation test, the Forbes–Rigobon heteroskedasticity correction (2002), and a complementary treatment of dynamic dependence, this study seeks to adjudicate a precise question: does the rise in asset co-movement observed during periods of crisis reflect genuine financial contagion — a structural alteration in the transmission mechanism of shocks — or is it a statistical artefact mechanically produced by the surge in variance? The distinction is not semantic: it bears directly on the validity of the risk-management models (VaR, regulatory stress tests) that presuppose a stable covariance structure.

---

## 1. Introduction and Epistemological Positioning

The injunction to diversify one's portfolio belongs to the register of received wisdom: it circulates as a commonsense theorem, seldom questioned precisely because it is seldom formulated with the precision its verification would require. Yet the theory underpinning it — Markowitz's mean-variance framework — does not merely state that one should hold weakly correlated assets; it tacitly assumes that this low correlation is a *structural* property of the market, rather than a *conjunctural* feature that holds only so long as nothing untoward occurs.

This project departs from a deliberate methodological refusal: that of accepting, out of intellectual deference, a premise that has never been subjected to a genuine test of falsifiability. The approach adopted here is not one of pedagogical illustration seeking to confirm an intuition, but an empirical trial whose outcome — genuine contagion, statistical artefact, or an intermediate configuration — is determined only *a posteriori*, by the data itself. The interpretive framework (Section 4.4) was fixed prior to the execution of the analysis, precisely to guard against the confirmation bias that would otherwise arise from reinterpreting results once they are known.

## 2. Research Question and Formalised Hypotheses

**Research question.** Does the dependence structure among risky assets — and consequently the risk-reduction benefit that diversification is purported to confer — remain stable during a period of market stress, or does it deteriorate precisely at the moment when the protection it promises is most sought?

Formally, for each asset pair (i, j), let ρ_N denote the normal-period correlation, ρ_C the crisis-period correlation, and ρ*_C the crisis-period correlation corrected for variance-induced inflation (Forbes–Rigobon).

- **H0 (null hypothesis):** ρ*_C = ρ_N, up to sampling noise — that is, once the mechanical effect of heteroskedasticity has been neutralised, no structural change in dependence is detectable.
- **H1 (alternative hypothesis):** ρ*_C > ρ_N in a statistically significant manner — dependence between assets genuinely intensifies under stress, independently of the variance effect.

Applying a bias correction prior to hypothesis testing is precisely what distinguishes this approach from the anecdotal inference — statistically naïve — that "correlations rose, therefore diversification failed."

## 3. Theoretical Framework

### 3.1 The Markowitzian Paradigm and Its Implicit Stationarity Assumption

The mean-variance framework (Markowitz, 1952) expresses the variance of a two-asset portfolio as:

σ_p² = w₁²σ₁² + w₂²σ₂² + 2w₁w₂σ₁σ₂ρ₁,₂

The diversification benefit derives entirely from the covariance term: the lower the correlation ρ, the further the portfolio's total risk falls below the weighted average of its constituent risks. Generalised to N assets via the covariance matrix Σ, this principle underlies the efficient frontier and, by extension, the bulk of contemporary asset management — including the parametric Value-at-Risk models deployed by financial institutions. The framework's blind spot is that it treats ρ as a fixed scalar, estimated once and assumed invariant to the state of the world — a **weak stationarity** assumption regarding the dependence structure that Markowitz himself never theoretically justified: it began as a computational convenience and has, through pedagogical sedimentation, hardened into an implicit presupposition.

### 3.2 The Correlation-Breakdown Literature

**Longin & Solnik (2001)** deploy extreme value theory — the statistical study of distributional tails rather than their central mass — to demonstrate that correlation among international equity markets rises asymmetrically during extreme joint downside movements, with no symmetric counterpart on the upside. This finding implies that estimating a single correlation coefficient over a full historical sample structurally understates co-movement risk in precisely the downside scenarios where protection is sought.

**Forbes & Rigobon (2002)** supply the methodological keystone of this project: the Pearson correlation coefficient is not invariant to the level of variance in the sample from which it is computed. Any rise in volatility — mechanical during a crisis — biases the correlation estimator upward, independently of any genuine change in the underlying dependence. They derive a heteroskedasticity-adjusted estimator:

ρ* = ρ / √(1 + δ(1 − ρ²))

where δ denotes the proportional increase in variance between the two periods under comparison. This correction reframes the empirical question in falsifiable terms: once the artefact has been stripped out, does an excess of dependence still remain?

**Ang & Chen (2002)** shift the angle of attack: rather than comparing two calendar periods, they condition correlation on the sign of joint returns. Under the (implicitly Markowitzian) assumption of multivariate normality, correlation should be identical during joint upside and joint downside movements, the multivariate normal distribution being symmetric about its mean. Ang and Chen document a clear and systematic violation: downside conditional correlation substantially exceeds upside conditional correlation — a result incompatible with the Gaussian assumption, and one suggesting that lower-tail dependence is structurally underestimated by classical models.

These three contributions cohere into a cumulative argument: Longin & Solnik establish the empirical asymmetry; Ang & Chen show it to be a recurring distributional property rather than an isolated accident; Forbes & Rigobon supply the statistical instrument needed to determine, for any given episode, how much of that asymmetry reflects genuine contagion rather than measurement artefact.

### 3.3 Beyond Static Correlation: Modelling Dynamic Dependence

Comparing correlations computed over disjoint windows (before/during/after) suffers from an intrinsic limitation: it yields only an averaged snapshot of each regime and says nothing about the *dynamics* of adjustment — the speed at which dependence recomposes itself at the onset of crisis. To move beyond this limitation, the project incorporates two econometric complements:

**a) Dynamic Conditional Correlation (DCC-GARCH, Engle 2002).** The DCC-GARCH model estimates, at each instant t, a conditional correlation matrix R_t evolving according to an autoregressive process, after each return series has been filtered through a univariate GARCH(1,1) model to normalise conditional volatility. Formally, letting ε_t = D_t⁻¹r_t denote the standardised returns (D_t being the diagonal matrix of conditional standard deviations), dynamic correlation is given by:

Q_t = (1 − α − β)Q̄ + α(ε_{t-1}ε'_{t-1}) + βQ_{t-1}, then R_t = diag(Q_t)^{-1/2} Q_t diag(Q_t)^{-1/2}

This model yields a continuous correlation trajectory around the crisis rather than a binary comparison of two averages, and serves as a **cross-validation** of the Forbes–Rigobon result: convergence between two methodologically independent approaches substantially strengthens confidence in the finding.

**b) Copula Analysis and Asymptotic Tail Dependence.** Sklar's theorem (1959) allows the dependence structure between two variables to be disentangled from their respective marginal distributions. By fitting an elliptical copula (Student-t) or an extreme-value copula (Gumbel, Clayton) to the asset pairs under study, one can estimate a lower tail-dependence coefficient λ_L — the probability that one asset collapses sharply *given* that the other has done so, in the limit of extreme quantiles. Unlike Pearson correlation, this coefficient specifically captures the risk of simultaneous joint collapse, which is the actual object of concern for an investor engaged in risk management.

### 3.4 Microfounded Economic Mechanisms

Three transmission channels, not mutually exclusive, are invoked to interpret any residual excess of dependence surviving correction:

1. **Flight to quality.** Under stress, investors cease finely arbitraging on individual fundamentals and retreat collectively toward a narrow set of safe-haven assets (sovereign debt, gold, cash), compressing the return distribution of "everything risky" into a common downward movement independent of sectoral fundamentals.

2. **Liquidity spirals and forced selling (Brunnermeier & Pedersen, 2009).** Funds facing margin calls or mass redemptions are compelled to liquidate their most liquid holdings — not necessarily their most fundamentally fragile ones — creating a purely mechanical transmission channel between assets with no genuine economic relationship. This mechanism generates a **margin spiral**: falling prices raise collateral requirements, forcing further sales that amplify the initial decline in a self-reinforcing loop.

3. **Statistical inflation via variance (Forbes & Rigobon, 2002).** The only purely metrological mechanism among the three: a volatility shock mechanically inflates the correlation estimator without requiring any genuine change in underlying dependence. This is the sole channel the Forbes–Rigobon correction is designed to neutralise.

The distinction between contagion (mechanisms 1 and 2, involving a structural change in the data-generating process) and measured excess interdependence (mechanism 3, an estimator artefact) revives the debate initiated by King & Wadhwani (1990) on the international transmission of equity shocks — a debate that Forbes & Rigobon would formalise statistically a decade later.

## 4. Methodology

### 4.1 Asset Selection and Economic Rationale

| Ticker | Role | Rationale for Inclusion |
|---|---|---|
| AAPL | Technology | High intra-sectoral baseline correlation with MSFT — the least favourable case for diversification even under calm conditions |
| MSFT | Technology | Intra-sectoral comparison pair with AAPL |
| XOM | Energy | Historically low correlation with technology — tests whether the breakdown crosses sectoral boundaries |
| JNJ | Healthcare (defensive) | Traditionally weakly correlated with the two preceding sectors — tests the resilience of defensive assets |
| GLD | Gold (safe haven) | Reference asset for flight-to-quality dynamics — tests whether the correlation breakdown is universal or specific to risky assets |

### 4.2 A Three-Regime Quasi-Experimental Design

| Regime | Window | Function in the Design |
|---|---|---|
| Normal | January 2017 – December 2019 | Establishes the baseline dependence structure |
| Crisis | February 2020 – April 2020 | Treatment window — a precisely dated exogenous shock |
| Control | January 2021 – December 2023 | Verifies that any observed break is specific to the crisis episode rather than indicative of a permanent structural shift (temporal placebo) |

**Sensitivity analysis on the temporal partition.** To pre-empt the objection that the exact boundaries chosen (February–April 2020) might be arbitrary, the protocol re-estimates ρ* while shifting the crisis window's boundaries by ± 10 trading days in each direction. If the qualitative result (significance of the break) remains stable under this perturbation, it cannot be attributed to the particular partition chosen.

### 4.3 Complete Econometric Pipeline

1. **Data acquisition**: daily adjusted closing prices via `yfinance`.
2. **Transformation into log-returns** r_t = ln(P_t/P_{t-1}), which are time-additive and better approximated by a normal distribution over short horizons.
3. **Jarque–Bera normality test** on each return series, to quantify excess kurtosis explicitly rather than merely noting it as an unaddressed caveat. The result conditions the interpretation of the subsequent Fisher test's p-value.
4. **Ljung–Box test** on returns and their squares, to verify the absence of residual autocorrelation and confirm the presence of volatility clustering that justifies a GARCH specification.
5. **Correlation and covariance matrices** (Pearson) computed by regime, supplemented by **Spearman and Kendall** coefficients as a robustness check — both less sensitive to crisis-period outliers than Pearson, neutralising that particular objection.
6. **Fisher z-transformation test** on each asset pair, formally comparing ρ_N and ρ_C via the statistic z = (z_C − z_N) / √(1/(n_C−3) + 1/(n_N−3)), where z = ½ln((1+ρ)/(1−ρ)).
7. **Forbes–Rigobon correction** yielding ρ*_C, the only quantity legitimately comparable to ρ_N.
8. **Multiple-comparisons correction (Benjamini–Hochberg, 1995)** applied across all p-values obtained over the ten asset pairs, controlling the False Discovery Rate — absent from the initial protocol, and necessary whenever several tests are conducted simultaneously.
9. **Block bootstrap confidence intervals** for ρ*: given that the crisis window comprises only around 60 observations, a non-parametric confidence interval obtained by resampling contiguous blocks (preserving temporal autocorrelation) is preferable to a Gaussian asymptotic approximation.
10. **Complementary DCC-GARCH estimation** (Section 3.3-a) to visualise a continuous conditional-correlation trajectory around the break point.
11. **Copula-based tail-dependence estimation** (Section 3.3-b) to quantify λ_L independently of linear correlation.
12. **Translation into portfolio-level risk**: volatility of an equal-weighted portfolio computed under the normal- versus crisis-period covariance matrix, with and without the Forbes–Rigobon correction, quantifying the risk that a static allocation would have failed to anticipate.

### 4.4 Pre-Registered Interpretive Framework

| Observed Configuration | Interpretation |
|---|---|
| Fisher test significant AND the gap survives the Forbes–Rigobon correction AND DCC-GARCH converges to the same conclusion | Genuine contagion: the dependence structure truly changes under crisis conditions |
| Fisher test significant BUT the gap largely vanishes after correction | The observed correlation spike is predominantly a variance-driven statistical artefact — a methodological nuance frequently absent from popular financial commentary |
| Elevated λ_L (tail dependence) despite a stable ρ* | A signal of non-linear contagion invisible to Pearson correlation but detectable via copulas — the subtlest and most informative case |
| GLD's correlation with equities remains low or negative | Flight to quality operates at the cross-asset-class level even as intra-equity diversification deteriorates |

## 5. Data Provenance and Official Archival Sources

Rather than leaving the provenance of the time series implicit, this section documents precisely where each data input originates, as a matter of replicability rather than mere formality.

**Primary source — Yahoo Finance, accessed via the `yfinance` API.** This supplies the daily adjusted closing prices for the five selected tickers (AAPL, MSFT, XOM, JNJ, GLD) across all three windows (2017–2019, 2020, 2021–2023). Adjusted closes are used specifically because they incorporate dividends and stock splits, which is a necessary condition for log-returns to correctly reflect total return rather than price return alone.

**External benchmark — FRED (Federal Reserve Bank of St. Louis), series `VIXCLS`** (https://fred.stlouisfed.org/series/VIXCLS), the CBOE Volatility Index. This series exists independently of the five equities under study and serves to objectively confirm the timing and magnitude of the volatility surge over February–April 2020, rather than relying on visual inspection of the very asset prices whose correlation is under investigation.

**External corroboration — FRED, series `BAMLH0A0HYM2`** (https://fred.stlouisfed.org/series/BAMLH0A0HYM2), the ICE BofA US High Yield Option-Adjusted Spread. A sharp widening of this spread during the crisis window corroborates, from the credit market rather than the equity market, that the episode selected constitutes a genuine systemic liquidity event rather than an equity-specific fluctuation.

**Cross-validation archive — CBOE DataShop** (https://datashop.cboe.com) **and S&P Dow Jones Indices** (https://www.spglobal.com/spdji/en/). These official repositories are held in reserve for higher-frequency or sector-level volatility validation, should the daily-frequency series prove insufficient to confirm a finding at a finer temporal resolution.

The methodological point of introducing VIXCLS and BAMLH0A0HYM2 goes beyond mere traceability: because both series are external to the five assets under study, they allow the crisis window to be dated **independently** of the very data on which the correlation tests are run — avoiding the circular reasoning of defining the crisis period by reference to the assets whose increased correlation is precisely what is being investigated.

## 6. Numerical and Economic Illustrations

The protocol above is statistically dense; the three vignettes below make its underlying mechanism concrete on simple worked or qualitative cases, without pre-empting the actual findings of Section 7.

### Vignette A — The Forbes–Rigobon Trap, Worked Numerically

Let the true, stable correlation between two assets be ρ = 0.40. Suppose that during the crisis period, return variance triples (i.e., δ = 2, where 1+δ is the variance multiplier). Even with no genuine change whatsoever in the underlying dependence between the two assets, the naïve correlation estimator computed over the crisis period yields:

ρ_observed = ρ·√(1+δ) / √(1+δρ²) = 0.40 × √3 / √(1+2×0.16) = 0.693 / 1.149 ≈ **0.60**

An unwary reader observing this jump from 0.40 to 0.60 would conclude that dependence between the two assets had risen by roughly half. Applying the Forbes–Rigobon correction in reverse:

ρ* = ρ_observed / √(1+δ(1−ρ_observed²)) = 0.60 / √(1+2×(1−0.36)) = 0.60 / 1.508 ≈ **0.40**

recovers the true correlation exactly: the entirety of the apparent increase was a mechanical artefact of variance, with no genuine contagion whatsoever. This is precisely the configuration the interpretive framework (Section 4.4) labels a "statistical artefact."

*A note on rigour: reaching a jump as large as 0.85 from a true correlation of 0.40 would require variance to increase roughly fourteen-fold, not merely double — the figures above were chosen to remain algebraically verifiable rather than dramatic.*

### Vignette B — Economic Expectations by Asset Pair

**AAPL / MSFT (intra-sectoral).** Correlation is expected to be already elevated under normal conditions, with a further increase anticipated during crisis. Both firms are exposed to the same sectoral risk factors — discount-rate sensitivity of future cash flows, sentiment toward growth valuations — making this the least favourable case for diversification even before any crisis occurs.

**AAPL / XOM (inter-sectoral).** Normal-period correlation is expected to be comparatively low; a crisis-period increase, if observed, would be more informative here than in the AAPL/MSFT pair, since a rise between two a priori unrelated sectors (technology and energy) would point toward a dominant common factor — liquidity, panic-driven selling — rather than a sector-specific effect.

**AAPL / GLD (safe-haven pairing).** Correlation is expected to remain low or negative, ideally stable or even declining during the crisis. Gold is theorised to benefit from the flight-to-quality mechanism (Section 3.4): if its correlation with equities fails to track the general increase observed elsewhere, this would confirm that the correlation breakdown remains confined to risky assets rather than being universal across asset classes.

This is not a set of results — it is a set of predictions stated *before* execution, so that the actual findings (Section 7) can be checked explicitly against an expectation formulated in advance rather than rationalised after the fact.

### Vignette C — Stable Correlation, Rising Tail Dependence: The "Black Swan" Case

It is possible for a Fisher/Forbes–Rigobon test to detect *no* significant break in ρ* even though the risk of joint collapse has genuinely worsened. Illustrative example: ρ* remains stable at 0.35 both before and during the crisis (no signal via linear correlation), yet the lower tail-dependence coefficient λ_L, estimated via copula, rises from 0.05 in the normal regime to 0.40 in the crisis regime. Two assets that appeared statistically independent in their ordinary day-to-day movements collapse jointly in extreme scenarios — a risk invisible to average correlation, but decisive for an investor exposed to tail risk. This is the subtlest configuration in the interpretive framework (Section 4.4), and the principal justification for incorporating copulas into the protocol rather than relying on Forbes–Rigobon alone.

*The numerical values in this vignette are illustrative, intended to make the mechanism concrete — they are not findings obtained from real data.*

## 7. Current Status and Results Protocol

At this stage, the methodological protocol is fully specified and the `main_analysis.py` script is ready for execution, but **no numerical results have yet been produced on real data**. This section therefore constitutes a reporting template, structured to receive the pipeline's outputs directly upon execution:

- Table of mean pairwise correlations by regime (ρ_N, ρ_C, ρ*_C)
- Table of Fisher test statistics and Benjamini–Hochberg-corrected p-values
- 95% bootstrap confidence intervals for each ρ*_C
- DCC-GARCH trajectory plots for the most significant pairs
- Copula-based λ_L estimates for each pair
- Portfolio volatility gap (normal vs. crisis, corrected vs. uncorrected)

*No figures are reported here until they derive from an actual execution of the pipeline — this is a non-negotiable condition of the project's integrity.*

## 8. Limitations and Methodological Solutions Implemented

| Identified Limitation | Solution Built Into the Protocol |
|---|---|
| Short crisis sample (~60 days), potentially wide confidence intervals | Block bootstrap (Section 4.3, step 9) rather than asymptotic Gaussian approximation |
| Multiple testing across ten asset pairs → risk of false positives | Benjamini–Hochberg (FDR) correction applied systematically (step 8) |
| Pearson's sensitivity to crisis-period extreme values | Cross-validation via Spearman and Kendall coefficients (step 5) |
| Arbitrary choice of crisis-window boundaries | Sensitivity analysis via ± 10-day boundary shifts (Section 4.2) |
| Absence of dynamic modelling (comparison of fixed averages) | Addition of DCC-GARCH (Section 3.3-a) for a continuous trajectory |
| Excess kurtosis noted but untested | Systematic Jarque–Bera testing (step 3) |
| Linear correlation insensitive to non-linear tail dependence | Addition of copula analysis and λ_L estimation (Section 3.3-b) |
| Study confined to a single crisis episode (uncertain generalisability) | Remains an acknowledged limitation of the current scope — addressed as a future direction (Section 8), not resolved here, since resolution would require full replication across 2008 and 2022 |

The final point is deliberately left unresolved, as a matter of intellectual honesty: adding finer statistical instruments strengthens the study's internal validity with respect to the COVID-19 episode, but does not settle the question of external validity (generalisation to other crisis typologies), which calls for an expansion of the project's scope rather than a refinement of its instrumentation.

## 9. Conclusion (Scope of the Protocol, Pending Results)

This work does not, at this stage, purport to settle the question it poses: it guarantees only its falsifiability. The contribution of this project lies not in an empirical result — non-existent until the pipeline has run on real data — but in the construction of a protocol capable of distinguishing, with a rigour not easily contested, genuine contagion from measurement artefact, two phenomena that popular financial commentary routinely conflates. Whatever the empirical outcome — confirmation of H1 (genuine contagion), confirmation of H0 (statistical artefact), or an intermediate configuration (non-linear dependence detected via copulas despite the absence of a linear-correlation signal) — each of these three outcomes would constitute a substantial, publishable finding in its own right, which is itself the mark of a properly specified research design.

## 10. Future Directions

- Full replication of the protocol across the 2008 Global Financial Crisis and the 2022 rate-hike sell-off, to test whether correlation breakdown is an invariant feature of market crises or a specificity of liquidity-driven shocks such as COVID-19.
- Extension to a backtested minimum-variance portfolio, comparing realised crisis-period risk against the risk anticipated at the time of initial allocation.
- Broadening the sample to a wider sectoral and geographic panel, to test the robustness of the correlation breakdown beyond the five currently selected assets.
- Modelling a Markov-switching regime to endogenise the dating of the break point rather than fixing it *a priori* at the crisis's official boundaries.

## References

- Ang, A., & Chen, J. (2002). *Asymmetric Correlations of Equity Portfolios*. Journal of Financial Economics, 63(3), 443–494.
- Benjamini, Y., & Hochberg, Y. (1995). *Controlling the False Discovery Rate*. Journal of the Royal Statistical Society, Series B, 57(1), 289–300.
- Brunnermeier, M. K., & Pedersen, L. H. (2009). *Market Liquidity and Funding Liquidity*. Review of Financial Studies, 22(6), 2201–2238.
- Engle, R. (2002). *Dynamic Conditional Correlation: A Simple Class of Multivariate GARCH Models*. Journal of Business & Economic Statistics, 20(3), 339–350.
- Forbes, K. J., & Rigobon, R. (2002). *No Contagion, Only Interdependence: Measuring Stock Market Comovements*. The Journal of Finance, 57(5), 2223–2261.
- King, M. A., & Wadhwani, S. (1990). *Transmission of Volatility between Stock Markets*. Review of Financial Studies, 3(1), 5–33.
- Longin, F., & Solnik, B. (2001). *Extreme Correlation of International Equity Markets*. The Journal of Finance, 56(2), 649–676.
- Markowitz, H. (1952). *Portfolio Selection*. The Journal of Finance, 7(1), 77–91.
- Sklar, A. (1959). *Fonctions de répartition à n dimensions et leurs marges*. Publications de l'Institut de Statistique de l'Université de Paris, 8, 229–231.
