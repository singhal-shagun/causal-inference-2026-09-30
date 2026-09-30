---
type: Lesson
title: "Lesson 35 — Inverse Probability Weighting and Doubly Robust Methods"
description: "Building a randomized trial out of the data: IPW identification and estimators, weight trimming, doubly robust estimation, other methods (matching, double ML, causal forests), and confidence intervals."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: teacher/1.0, at: 2026-09-17 }
---

# Lesson 35: Inverse Probability Weighting and Doubly Robust Methods

## Where We Left Off

Lesson 34 introduced the propensity score $e(w) = P(T = 1 \mid W = w)$. This lesson puts it to work. Neal frames the motivating question this way: what if the data could be reweighted so that association *becomes* causation? The answer is **inverse probability weighting** (IPW). Both of this course's sources treat it: Primer §3.6 works through it on a table of numbers, and Neal's Chapter 7 turns it into an estimator.

*Notation.* As in Lessons 33–34: $T$ is a binary treatment, $Y$ the outcome, $W$ a sufficient adjustment set. The Primer writes $X$ and $Z$ for what is here $T$ and $W$.

## The Idea: Reweighting Away Confounding

In the confounded graph $W \to T$, $W \to Y$, $T \to Y$, association is not causation because treatment depends on the covariates: $P(T \mid W) \neq P(T)$. Suppose the data could be reweighted into a **pseudo-population** in which treatment no longer depends on $W$. In the graph of that pseudo-population the arrow $W \to T$ would be gone, and all remaining association between $T$ and $Y$ would be causal, exactly as in a randomized experiment.

The propensity score provides the weights. Give each unit a weight equal to **the inverse of the probability that it received the treatment it actually received**, given its covariates:

- a treated unit gets weight $1 / e(w)$;
- an untreated unit gets weight $1 / (1 - e(w))$.

**Why this works, intuitively.** Consider a stratum of $W$ where treatment is rare, say $e(w) = 0.2$. There are few treated units there, and each is given weight 5, so that together they stand in for the whole stratum under treatment. The many untreated units each get weight $1/0.8 = 1.25$, so that together they stand in for the whole stratum under no treatment. After weighting, each stratum contributes to the treated arm and to the untreated arm in the same proportion: its share of the population. That is the balance a randomized experiment would have produced.

## Identification

For binary treatment, IPW identifies each potential-outcome mean:

$$\mathbb{E}[Y(1)] = \mathbb{E}\left[ \frac{\mathbb{1}(T = 1)\, Y}{e(W)} \right].$$

**Derivation.** Let $\mathbb{1}(T = 1)$ be 1 for treated units and 0 otherwise.

$$
\begin{aligned}
\mathbb{E}\left[ \frac{\mathbb{1}(T = 1)\, Y}{e(W)} \right]
&= \mathbb{E}\left[ \frac{\mathbb{E}[\mathbb{1}(T = 1)\, Y \mid W]}{e(W)} \right] && \text{law of total expectation, conditioning on } W \\
&= \mathbb{E}\left[ \frac{\mathbb{E}[\mathbb{1}(T = 1)\, Y(1) \mid W]}{e(W)} \right] && \text{consistency: } Y = Y(1) \text{ when } T = 1 \\
&= \mathbb{E}\left[ \frac{\mathbb{E}[\mathbb{1}(T = 1) \mid W]\; \mathbb{E}[Y(1) \mid W]}{e(W)} \right] && \text{unconfoundedness: } T \perp\!\!\!\perp Y(1) \mid W \\
&= \mathbb{E}\left[ \frac{e(W)\; \mathbb{E}[Y(1) \mid W]}{e(W)} \right] && \mathbb{E}[\mathbb{1}(T = 1) \mid W] = e(W) \\
&= \mathbb{E}\big[ \mathbb{E}[Y(1) \mid W] \big] = \mathbb{E}[Y(1)] && \text{cancel, then total expectation}
\end{aligned}
$$

The cancellation needs $e(W) > 0$, which is positivity. The same steps with $1 - e(W)$ give $\mathbb{E}[Y(0)]$, and subtracting yields the average treatment effect:

$$\mathbb{E}[Y(1) - Y(0)] = \mathbb{E}\left[ \frac{\mathbb{1}(T = 1)\, Y}{e(W)} - \frac{\mathbb{1}(T = 0)\, Y}{1 - e(W)} \right].$$

The right-hand side involves only observable quantities, so it is a statistical estimand.

## The IPW Estimator

Replace the expectation with an average over the data and the true propensity with a fitted model $\widehat{e}(w)$:

$$\widehat{ATE} = \frac{1}{n} \sum_{i=1}^{n} \left[ \frac{\mathbb{1}(T_i = 1)\, Y_i}{\widehat{e}(w_i)} - \frac{\mathbb{1}(T_i = 0)\, Y_i}{1 - \widehat{e}(w_i)} \right].$$

This estimator goes back to Horvitz and Thompson (1952). A CATE version averages only over the units with $X = x$, and can suffer from high variance when there are few of them.

## Pearl's Worked Tables

The Primer applies the same reweighting to the drug data of Lesson 19, written as a joint distribution. Here $T$ is taking the drug, $Y$ recovery and $W$ gender.

| $T$ (drug) | $Y$ (recovered) | $W$ (gender) | Share of population |
| :---: | :---: | :---: | :---: |
| Yes | Yes | Male | 0.116 |
| Yes | Yes | Female | 0.274 |
| Yes | No | Male | 0.010 |
| Yes | No | Female | 0.101 |
| No | Yes | Male | 0.334 |
| No | Yes | Female | 0.079 |
| No | No | Male | 0.051 |
| No | No | Female | 0.036 |

*Primer Table 3.3, the Table 1.1 data as proportions of the 700 patients.*

**Conditioning is uniform reweighting.** To condition on "took the drug", drop the untreated rows and multiply the remaining ones by the same constant, $1 / P(T = \text{yes})$. Here $P(T = \text{yes}) = 0.116 + 0.274 + 0.010 + 0.101 = 0.501$, essentially $350/700 = 0.5$, so each row is doubled. (The Primer's text prints 0.49 and a factor of 2.041, but its Table 3.4 values, such as $0.116 \times 2 = 0.232$, correspond to 0.5.) The result (Primer Table 3.4) keeps the drugged group's gender mix: about 75% female ($0.547 + 0.202 = 0.749$). The female-heavy mix is what produced Simpson's reversal.

**IPW is stratum-specific reweighting.** Instead, multiply each drugged row by $1 / P(T = \text{yes} \mid W)$ for its own gender. From the table:

$$
\begin{aligned}
P(T = \text{yes} \mid \text{male}) &= \frac{0.116 + 0.010}{0.116 + 0.010 + 0.334 + 0.051} = \frac{0.126}{0.511} = 0.247 \\
P(T = \text{yes} \mid \text{female}) &= \frac{0.274 + 0.101}{0.274 + 0.101 + 0.079 + 0.036} = \frac{0.375}{0.490} = 0.765
\end{aligned}
$$

(From the unrounded counts of Table 1.1, the male figure is $87/357 = 0.244$. The Primer prints 0.233, which is a misprint: its own Table 3.5 and its stated weighting factor of 4.1 both correspond to 0.244.)

So drugged men are weighted by about $1/0.244 = 4.1$ and drugged women by about $1/0.765 = 1.3$:

| $T$ | $Y$ | $W$ | Share of pseudo-population |
| :---: | :---: | :---: | :---: |
| Yes | Yes | Male | 0.476 |
| Yes | Yes | Female | 0.357 |
| Yes | No | Male | 0.041 |
| Yes | No | Female | 0.132 |

*Primer Table 3.5: the population under $do(T = \text{yes})$, obtained by inverse probability weighting.*

For example, the first row is $0.116 / 0.244 = 0.476$ and the second is $0.274 / 0.765 = 0.358$. No separate renormalization is needed: the weighted shares already sum to about 1, because the men's rows now add up to the men's share of the population (0.51) and the women's rows to the women's share (0.49). Then

$$P(Y = \text{yes} \mid do(T = \text{yes})) = 0.476 + 0.357 = 0.833,$$

which matches the adjustment formula computed from the unrounded rates, $0.51 \times 0.931 + 0.49 \times 0.730 = 0.833$.

**The same formula as Lesson 19.** Lesson 19 derived $P(w, y \mid do(t)) = P(t, y, w) / P(t \mid w)$. That equation *is* inverse probability weighting: the numerator is the observed joint distribution, the denominator is the propensity, and dividing converts each observed case into its share of the interventional world. The difference from conditioning is only in the weights. Conditioning divides every row by the same constant $P(t)$; IPW divides each row by its own stratum's $P(t \mid w)$, and that variation is what removes the dependence of treatment on the covariates.

> [!IMPORTANT]
> **IPW builds a randomized trial out of observational data.** The reweighted table is the manipulated world of Lesson 18, in which the arrow $W \to T$ has been cut. Two conditions follow from this picture. **Positivity**: every stratum must contain both treated and untreated units, or the weight $1/e(w)$ is undefined. **A valid adjustment set**: the variables in $W$ must satisfy the backdoor criterion. Weighting on the wrong variables builds a randomized trial of the wrong experiment, and can introduce more bias than no adjustment at all.

## Practical Problems with IPW

**Extreme weights.** When a propensity is close to 0 or 1, some units get very large weights, and the estimate is dominated by a handful of them. The estimator's variance becomes very large even if it is correctly specified: its confidence interval may be too wide to be useful. A common remedy, noted by Neal, is **weight trimming**: raise any propensity below a small threshold $\alpha$ to $\alpha$, and lower any above $1 - \alpha$ to $1 - \alpha$, which caps every weight at $1/\alpha$. This reduces variance at the cost of some bias.

**When IPW helps.** The Primer's discussion of precision points out an advantage of IPW over stratified adjustment. With many covariates there may be thousands of strata but only hundreds of units, so most strata are empty and the adjustment formula cannot be computed directly. IPW needs only one propensity per *unit*, so it deals with only as many distinct values as there are observations. The propensity model still has to be fitted to all of $W$, though, so the difficulty moves into that model, as Lesson 34 warned.

## Doubly Robust Methods

Lesson 33 modelled the outcome, $\mu(t, w) = \mathbb{E}[Y \mid T = t, W = w]$. Lesson 34 and this lesson model the treatment, $e(w)$. Why not model both? Estimators that do so can be **doubly robust**.

> **Double robustness.** A doubly robust estimator is consistent if *either* the outcome model $\widehat{\mu}$ is a consistent estimator of $\mu$ *or* the propensity model $\widehat{e}$ is a consistent estimator of $e$. Only one of the two needs to be right.

The best-known example is the **augmented inverse probability weighted (AIPW)** estimator:

$$
\widehat{ATE}_{\text{AIPW}} = \frac{1}{n} \sum_{i=1}^{n} \left[ \widehat{\mu}_1(w_i) - \widehat{\mu}_0(w_i) + \frac{\mathbb{1}(T_i = 1)\,\big(Y_i - \widehat{\mu}_1(w_i)\big)}{\widehat{e}(w_i)} - \frac{\mathbb{1}(T_i = 0)\,\big(Y_i - \widehat{\mu}_0(w_i)\big)}{1 - \widehat{e}(w_i)} \right].
$$

The first two terms are the outcome-model (GCOM) estimate of Lesson 33. The last two correct it using the weighted residuals of the outcome model.

**Why it is doubly robust.** Look at the treated part, $\widehat{\mu}_1(W) + \mathbb{1}(T{=}1)\,(Y - \widehat{\mu}_1(W)) / \widehat{e}(W)$, and take expectations given $W$.

- *If the outcome model is right*, $\widehat{\mu}_1 = \mu_1$. Among treated units with covariates $W$, the residual $Y - \mu_1(W)$ has mean zero, so the correction term has expectation zero whatever $\widehat{e}$ is. What remains is $\mu_1(W)$, whose average is $\mathbb{E}[Y(1)]$ by the adjustment formula.
- *If the propensity model is right*, $\widehat{e} = e$. Rearrange the treated part as $\mathbb{1}(T{=}1)\,Y / e(W) + \widehat{\mu}_1(W)\,\big(1 - \mathbb{1}(T{=}1)/e(W)\big)$. The first term is the IPW term, whose mean is $\mathbb{E}[Y(1)]$ by the derivation above. The second has mean zero whatever $\widehat{\mu}_1$ is, because $\mathbb{E}[\mathbb{1}(T{=}1) \mid W] = e(W)$.

The untreated part works the same way.

**Faster convergence.** Neal adds a second advantage: the rate at which a doubly robust estimator converges is the *product* of the rates of its two models. Flexible machine-learning models in high dimensions each converge more slowly than the ideal $n^{-1/2}$, so multiplying their errors helps considerably.

**Caveats, from Neal.** How well doubly robust methods work when *neither* model is right is disputed (Kang and Schafer, 2007, "Demystifying double robustness"), although flexible machine-learning models may change the picture. The estimators that currently perform best all model the outcome flexibly, unlike pure IPW. A family of doubly robust methods called **targeted maximum likelihood estimation (TMLE)** has performed well in estimation competitions. Neal treats doubly robust methods as largely beyond his book's scope and recommends Seaman and Vansteelandt (2018) as an introduction.

## Other Estimation Methods

Neal's estimation chapter ends with short notes on three widely used families that his book does not develop in detail. Each estimates the same statistical estimand as Lessons 33–35; they differ in how.

### Matching

**Matching** pairs each treated unit with one or more untreated units that have similar covariates, discards units without a good match, and compares outcomes within the matched set. Matching can be done on the raw covariates, on coarsened versions of them, or on the propensity score. The analyst also chooses a distance, how close counts as a match (exact matching requires identical covariates), and how many matches each unit may have. Stuart (2010) reviews the options.

**A check on familiar data.** Match each drug taker in Table 1.1 exactly on sex with non-takers of the same sex. Each treated man is compared with untreated men (recovery 0.867), and each treated woman with untreated women (0.688). Averaging over the *treated* units, of whom 87 are men and 263 women:

$$\frac{87}{350}(0.931 - 0.867) + \frac{263}{350}(0.730 - 0.688) = 0.249 \times 0.064 + 0.751 \times 0.043 \approx 0.016 + 0.032 = 0.048.$$

This is the effect on the treated computed in Lesson 30, 0.048. Matching treated units to controls naturally targets the ATT, because the average runs over the treated.

### Double Machine Learning

**Double (or debiased) machine learning** (Chernozhukov and colleagues) fits three models in two stages.

1. **Stage 1.** Fit a model that predicts the outcome $Y$ from the covariates $W$, giving $\widehat{Y}$. Fit a second model that predicts the treatment $T$ from $W$, giving $\widehat{T}$.
2. **Stage 2.** Compute the residuals $Y - \widehat{Y}$ and $T - \widehat{T}$: the parts of outcome and treatment that the covariates do not explain. Fit a model that predicts the outcome residual from the treatment residual. Its slope is the effect estimate.

**Why the residuals remove confounding.** Take a linear example: $T := aW + U_T$ and $Y := \delta T + bW + U_Y$, with independent noise. Then $\mathbb{E}[T \mid W] = aW$ and $\mathbb{E}[Y \mid W] = \delta a W + bW$. The residuals are

$$T - \mathbb{E}[T \mid W] = U_T, \qquad Y - \mathbb{E}[Y \mid W] = \delta U_T + U_Y.$$

Regressing the second on the first gives slope $\mathrm{Cov}(\delta U_T + U_Y,\ U_T) / \mathrm{Var}(U_T) = \delta$. The confounder's influence has been "partialled out" of both variables. With flexible models in Stage 1, the same idea works when the effect of $W$ is far from linear. In practice each Stage-1 model is fitted on one part of the data and used to predict on another (**cross-fitting**), so that overfitting does not leak into the residuals.

### Causal Trees and Forests

A **causal tree** (Athey and Imbens) splits the data again and again into subgroups whose treatment effects are similar. The leaves are groups of units with roughly the same effect, which makes the tree a CATE estimator (Lesson 33). A **causal forest** (Wager and Athey) averages many such trees, just as a random forest averages decision trees, and is part of a broader family called generalized random forests. These methods were built with a specific aim: to give valid confidence intervals for the estimated effects. Susan Athey's guest lecture in Neal's course covered this family.

## How Uncertain Is the Estimate?

Every estimate so far has been a single number. With finite data, a different sample would give a different number, and a **confidence interval** reports that sampling uncertainty. When arbitrary machine-learning models are allowed, valid intervals are hard to obtain. Neal describes two approaches.

**Bootstrapping.** Repeat the whole estimation many times, each time on a new sample of the same size drawn *with replacement* from the data:

1. Draw a resample of $n$ units from the $n$ observed units, with replacement.
2. Run the full estimator on it (fit the models, compute the effect).
3. Repeat, say, 1,000 times, keeping each estimate.
4. Take the 2.5th and 97.5th percentiles of the 1,000 estimates as an approximate 95% interval.

The catch: bootstrapped intervals are not always valid. A nominal 95% interval may contain the true value less than 95% of the time, especially with flexible models.

**Specialized models.** Restrict the estimator to a form whose uncertainty is understood. Linear regression is the simplest case. Double machine learning with a linear second stage also yields standard intervals, and causal forests were designed to.

## These Methods Are Not a Randomized Experiment

Every method in Part 5 removes confounding by the variables in $W$. None of them touches confounders that were not measured. A randomized experiment removes both kinds at once (Lesson 21); adjustment, weighting, matching and their machine-learning versions remove only the first. No amount of modelling skill changes that. Whether all confounders were measured is an assumption, and it cannot be tested from the data. That is why Part 6 turns to bounds and sensitivity analysis: ways to ask how much the conclusions depend on that assumption.

## Summary and Key Takeaways

1. **IPW** weights each unit by the inverse probability of the treatment it actually received, creating a pseudo-population in which treatment is independent of the covariates.
2. The identification $\mathbb{E}[Y(1)] = \mathbb{E}[\mathbb{1}(T{=}1)\, Y / e(W)]$ follows in five steps from total expectation, consistency, unconfoundedness and positivity.
3. On Pearl's drug table, IPW weights drugged men by about 4.1 and drugged women by about 1.3, and gives $P(Y = \text{yes} \mid do(T = \text{yes})) = 0.833$, matching the adjustment formula.
4. IPW is Lesson 19's formula $P(w, y \mid do(t)) = P(t, y, w)/P(t \mid w)$ applied case by case. Conditioning divides by a constant; IPW divides by a stratum-specific propensity.
5. IPW needs positivity and a valid adjustment set. Propensities near 0 or 1 produce extreme weights; trimming trades variance for bias.
6. **Doubly robust** estimators such as AIPW combine an outcome model and a propensity model, stay consistent if either is right, and converge at the product of the two models' rates.
7. **Other methods:** matching compares treated units with similar controls (exact matching on sex gives the ATT, 0.048); double machine learning regresses outcome residuals on treatment residuals; causal trees and forests estimate subgroup effects with valid intervals.
8. **Uncertainty:** bootstrapping gives confidence intervals that are not always valid; specialized models (linear, double ML with a linear second stage, causal forests) give intervals with guarantees.
9. None of these methods handles **unmeasured** confounding, which only randomization removes.

**Part 5 is complete. Next step:** Part 6 confronts what every method so far has assumed away: **unobserved confounding**. It covers bounds, sensitivity analysis, instrumental variables and difference-in-differences.

### Check Your Understanding

1. Using Table 3.3, compute $P(Y = \text{yes} \mid do(T = \text{no}))$ by inverse probability weighting. Then compute the average causal effect and compare it with Lessons 19 and 33.
2. Show that the weighted shares of Table 3.5 sum to $P(\text{male}) + P(\text{female}) = 1$, and explain why IPW, unlike conditioning, needs no renormalization.
3. In the AIPW estimator, suppose the propensity model is correct but the outcome model predicts $\widehat{\mu}_1(w) = 0$ for everyone. What does the estimator reduce to?
4. A reviewer complains that your IPW weights reach 40. What is the likely cause? What remedy does this lesson offer, and what does it cost?
5. Using exact matching on sex, but matching each *untreated* unit to treated units of the same sex, compute the effect on the untreated. Compare it with the ATT (0.048) and explain the difference.
6. In the double machine learning example, suppose Stage 1 used $\widehat{T} = 0$ for everyone (no model for treatment). What would the Stage 2 slope estimate, and why is it biased?

---

## Further Reading

*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 3, Section 3.6 (Tables 3.3–3.5 and the discussion of precision); Brady Neal, *Introduction to Causal Inference* (Dec 2020 draft), Chapter 7 (Inverse Probability Weighting, Doubly Robust Methods, Other Methods, Concluding Remarks: confidence intervals and comparison to randomized experiments); Horvitz & Thompson (1952); Stuart (2010), *Matching methods for causal inference: a review and a look forward*; Chernozhukov et al. (2018), *Double/debiased machine learning for treatment and structural parameters*; Athey & Imbens (2016), *Recursive partitioning for heterogeneous causal effects*; Wager & Athey (2018), *Estimation and inference of heterogeneous treatment effects using random forests*; Bang & Robins (2005); Kang & Schafer (2007); Seaman & Vansteelandt (2018).
