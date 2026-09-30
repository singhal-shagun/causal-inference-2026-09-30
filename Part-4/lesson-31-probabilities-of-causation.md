---
type: Lesson
title: "Lesson 31 — Probabilities of Causation: Attribution and the Courtroom"
description: "PN, PS and PNS; the cancer-patient and discrimination examples; Theorem 4.5.1, the bounds of Eq. 4.30, and the court-case calculation."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: teacher/1.0, at: 2026-09-17 }
---

# Lesson 31: Probabilities of Causation — Attribution and the Courtroom

## Where We Left Off

Lesson 30 asked how a treatment affects the people who chose it. This lesson asks a question that looks backwards: given that something happened, was it *caused* by a particular action? Such questions of **attribution** decide legal liability, shape personal regret, and guide how people learn from their decisions. They can only be stated with counterfactuals.

*Notation.* $X$ and $Y$ are binary. For a value $x$, $x'$ denotes the other value, and likewise $y$ and $y'$.

## Three Patients

**Example 4.4.3.** Ms Jones has breast cancer and chooses lumpectomy *plus* irradiation. Ten years later she is alive, with no recurrence. She asks: *Do I owe my life to irradiation?* Mrs Smith chose lumpectomy alone, and her tumour recurred a year later. She regrets: *I should have gone through irradiation.*

A randomized trial (Fisher et al., 2002, a 20-year follow-up) settles the population question. Adding irradiation to lumpectomy cut recurrence from 39% to 14%. But that is an average over many women. It does not say what happened, or would have happened, to Ms Jones or Mrs Smith. Their questions need counterfactuals. Let $Y = 1$ stand for remission (no recurrence) and $X = 1$ for irradiation.

**Ms Jones: the probability of necessity.**

$$PN = P(Y_0 = 0 \mid X = 1, Y = 1). \tag{4.23}$$

Given that she was irradiated and remission occurred, what is the probability that remission would *not* have occurred without irradiation? $PN$ measures how far her choice was **necessary** for the good outcome.

**Mrs Smith: the probability of sufficiency.**

$$PS = P(Y_1 = 1 \mid X = 0, Y = 0). \tag{4.24}$$

Given that she was not irradiated and the tumour recurred, what is the probability that remission *would* have occurred with irradiation? $PS$ measures how far the choice she did not make would have been **sufficient**.

**Ms Daily: the probability of necessity and sufficiency.** A third woman faces the same decision *before* treatment. She reasons: if my tumour would not recur even without irradiation, why suffer through it? If it would recur either way, why bother? The only reason to accept irradiation is if my tumour would remit *with* it and recur *without* it. Formally:

$$PNS = P(Y_1 = 1,\ Y_0 = 0). \tag{4.25}$$

| Quantity | Expression | Question it answers | Example |
| --- | --- | --- | --- |
| $PN$ (necessity) | $P(Y_0 = 0 \mid X = 1, Y = 1)$ | Would the outcome have failed to occur without the cause? | "Do I owe my life to irradiation?" |
| $PS$ (sufficiency) | $P(Y_1 = 1 \mid X = 0, Y = 0)$ | Would the cause have produced the outcome? | "Would irradiation have saved me?" |
| $PNS$ (both) | $P(Y_1 = 1, Y_0 = 0)$ | Does this case respond to the cause at all? | "Should I choose irradiation?" |

All three are cross-world. $PN$ and $PS$ condition on what happened and ask about a world where the choice was different; they *explain* an outcome. $PNS$ conditions on nothing and involves both worlds at once; it *predicts* who would benefit. None of them can in general be computed from experimental data alone, because an experiment never shows the same person in both worlds. As the rest of the lesson shows, combining experimental and observational data, sometimes with an extra assumption, often pins them down or bounds them.

Why care about backward-looking questions? Pearl gives two reasons. Regret and a sense of having been right are how people learn: confirming Ms Jones's success reinforces the way she made her decision, and Mrs Smith's regret points to weaknesses to fix. And the same quantities drive forward-looking decisions: Ms Daily's $PNS$ is exactly what she needs to choose.

## Discrimination and the Choice of Antecedent

**Example 4.4.4.** Mary sues XYZ International. She had the credentials for the job but was not hired, she says, because she mentioned in her interview that she is gay. U.S. courts define employment discrimination counterfactually: "The central question in any employment-discrimination case is whether the employer would have taken the same action had the employee been of a different race (age, sex, religion, national origin, etc.) and everything else had been the same" (*Carson v. Bethlehem Steel Corp.*, 1996). The criterion concerns the individual plaintiff, not a population, and its wording, "would have taken" and "had the employee been", is counterfactual.

The probability that Mary was not hired *because of* her orientation has the same form as Mrs Smith's question:

$$PS = P(Y_1 = 1 \mid X = 0, Y = 0),$$

where $Y = 1$ means being hired and $X$ is the **interviewer's perception** of Mary's orientation ($X = 0$: perceived as gay). It reads: the probability that Mary would have been hired had the interviewer perceived her as straight, given that the interviewer perceived her as gay and she was not hired.

The Primer explains the choice of *perception* rather than orientation itself. Intervening on the perception is easy to imagine: suppose Mary had simply not mentioned it. Imagining a change in Mary's actual orientation is formally possible but awkward, because it changes the person. The general point is that the antecedent of a counterfactual should be a variable that can be changed while everything else stays the same, and the causal graph is where that choice is made explicit. Although discrimination cannot be *proved* in an individual case, the probability that it occurred can be estimated, and it can sometimes come close to certainty.

## Identifying and Bounding the Probability of Necessity

In general form, for binary $X$ and $Y$:

$$PN(x, y) = P(Y_{x'} = y' \mid X = x, Y = y). \tag{4.27}$$

Given that $X = x$ and $Y = y$ both occurred, it is the probability that $Y$ would have been $y'$ had $X$ been $x'$. This is the legal "but for" test: a plaintiff should win if and only if it is "more probable than not" that the harm would not have occurred but for the defendant's action, that is, if $PN > \tfrac{1}{2}$.

### With Monotonicity: An Exact Formula

> **Theorem 4.5.1.** If $Y$ is monotonic relative to $X$, meaning $Y_1(u) \geq Y_0(u)$ for every unit $u$, then $PN$ is identifiable whenever the causal effect is, and
> $$PN = \frac{P(y) - P(y \mid do(x'))}{P(x, y)}. \tag{4.28}$$

**Monotonicity** says the treatment never *prevents* the outcome in anyone: there is no unit for whom $X = 1$ gives $Y = 0$ while $X = 0$ would give $Y = 1$.

Writing $P(y) = P(y \mid x)\, P(x) + P(y \mid x')\, P(x')$ and rearranging gives the equivalent form

$$PN = \underbrace{\frac{P(y \mid x) - P(y \mid x')}{P(y \mid x)}}_{\text{excess risk ratio (ERR)}} + \underbrace{\frac{P(y \mid x') - P(y \mid do(x'))}{P(x, y)}}_{\text{confounding factor (CF)}}. \tag{4.29}$$

- The **excess risk ratio** uses observational data only. Courts often rely on it when no experimental data exist. It is also known as the attributable risk fraction among the exposed.
- The **confounding factor** corrects for confounding. It compares how often $y$ occurs among those who *chose* $x'$, $P(y \mid x')$, with how often it would occur if *everyone* were assigned $x'$, $P(y \mid do(x'))$. If the two differ, the people who chose $x'$ are not typical, and the ERR alone is biased.

The Primer's illustration: in a suit against a car maker whose faulty design allegedly caused a death, the ERR measures how much more likely death is in that maker's cars. If people who buy those cars also drive faster, the confounding factor corrects for it.

### Without Monotonicity: Bounds

Without monotonicity, $PN$ is not identified, but Tian and Pearl (2000) showed it can be bounded using experimental and observational data together:

$$\max\left\{ 0,\ \frac{P(y) - P(y \mid do(x'))}{P(x, y)} \right\} \;\leq\; PN \;\leq\; \min\left\{ 1,\ \frac{P(y' \mid do(x')) - P(x', y')}{P(x, y)} \right\}. \tag{4.30}$$

The lower bound is the monotonic formula (4.28). The Primer notes three features of these bounds:

1. The width of the interval does not depend on confounding; it depends only on the observable ratio $P(y' \mid x) / P(y \mid x)$.
2. The confounding factor can lift the lower bound above $\tfrac{1}{2}$, meeting the "more probable than not" standard, when the ERR alone would not.
3. The only quantity needed from the experimental data is the one entering the confounding factor, $P(y \mid do(x'))$; the full causal effect is not needed.

### Also Under Monotonicity: $PNS$

With monotonicity, $PNS$ also follows from experimental data alone:

$$PNS = P(y \mid do(x)) - P(y \mid do(x')). \tag{4.42}$$

For Ms Daily, the trial gives remission rates of $1 - 0.14 = 0.86$ with irradiation and $1 - 0.39 = 0.61$ without. So $PNS = 0.86 - 0.61 = 0.25$: a 25% chance that her tumour is of the kind that remits with irradiation and recurs without it. The number relies on monotonicity, here the assumption that irradiation never causes a recurrence that would not otherwise have happened.

## A Court Case, Worked Out

**Example 4.5.1.** A lawsuit claims that drug $x$ caused the death of Mr A, who took it for back pain. The manufacturer presents experimental data showing that the drug barely changes death rates. The plaintiff replies that Mr A took the drug by his own choice, unlike the trial's subjects, and presents observational data on people who, like him, chose the drug. The Primer's hypothetical data:

| | Experimental: $do(x)$ | Experimental: $do(x')$ | Observational: took $x$ | Observational: did not ($x'$) |
| --- | :---: | :---: | :---: | :---: |
| Deaths ($y$) | 16 | 14 | 2 | 28 |
| Survivals ($y'$) | 984 | 986 | 998 | 972 |
| Total | 1,000 | 1,000 | 1,000 | 1,000 |

*Primer Table 4.5. In the observational study, half of the 2,000 people took the drug.*

The Primer's narrative says that voluntary takers "experienced higher death rates", but its table shows the opposite (2 per 1,000 against 28), and its calculation uses the table. This lesson follows the table.

**The estimates.**

$$
\begin{aligned}
P(y \mid do(x)) &= 16/1000 = 0.016, & P(y \mid do(x')) &= 14/1000 = 0.014, \\
P(y) &= (2 + 28)/2000 = 0.015, & P(x, y) &= 2/2000 = 0.001, \\
P(y \mid x) &= 2/1000 = 0.002, & P(y \mid x') &= 28/1000 = 0.028.
\end{aligned}
$$

**Under monotonicity** (the drug can cause death but never prevent it), use (4.29):

$$
\begin{aligned}
\text{ERR} &= \frac{0.002 - 0.028}{0.002} = \frac{-0.026}{0.002} = -13 \\
\text{CF} &= \frac{0.028 - 0.014}{0.001} = \frac{0.014}{0.001} = 14 \\
PN &= -13 + 14 = 1
\end{aligned}
\tag{4.41}
$$

The same answer comes directly from (4.28): $(0.015 - 0.014)/0.001 = 1$.

**Without monotonicity**, use the bounds (4.30):

$$
\begin{aligned}
\text{lower bound} &= \max\left\{0,\ \frac{0.015 - 0.014}{0.001}\right\} = \max\{0, 1\} = 1 \\
\text{upper bound} &= \min\left\{1,\ \frac{0.986 - 0.486}{0.001}\right\} = \min\{1, 500\} = 1
\end{aligned}
$$

where $P(y' \mid do(x')) = 986/1000 = 0.986$ and $P(x', y') = 972/2000 = 0.486$. Both bounds equal 1, so $PN = 1$ **without any assumption of monotonicity**.

**Reading the result.** The observational comparison alone points the other way: death was *rarer* among people who chose the drug, so the ERR is strongly negative. But those who chose *not* to take it died at a rate of 0.028, twice the rate of 0.014 that the experiment shows for people assigned not to take it. So the people who declined the drug were not typical: they were at higher risk. The confounding factor corrects for this, and once it does, the data leave no doubt that the drug caused Mr A's death.

Two lessons follow. First, attribution needs both kinds of data: the experiment supplies the causal effect of assignment, and the observational study supplies the pattern of self-selection. Second, unless monotonicity can be defended, the answer comes as a bound. Here monotonicity says the drug never *prevents* a death: no one who would have died without it survives because of it. Defending that in court means arguing about how the drug works, a claim a court can actually weigh.

## Summary and Key Takeaways

1. The three **probabilities of causation**: $PN$ (was the cause necessary for this outcome?), $PS$ (would the cause have produced the missing outcome?), and $PNS$ (does this case respond to the cause at all?).
2. All three are cross-world. In general none follows from experimental data alone; combining experimental and observational data identifies or bounds them.
3. **Theorem 4.5.1:** under monotonicity, $PN = [P(y) - P(y \mid do(x'))]/P(x, y)$, which splits into the excess risk ratio plus a confounding factor.
4. Without monotonicity, (4.30) bounds $PN$. The confounding factor can lift the lower bound past $\tfrac{1}{2}$.
5. Under monotonicity, $PNS = P(y \mid do(x)) - P(y \mid do(x'))$: 0.25 for Ms Daily.
6. In the court case, the observational data alone suggest the drug is protective, yet combining them with the experimental data gives $PN = 1$, even without monotonicity.
7. In legal questions, the antecedent should be a variable that can be changed while everything else stays fixed, such as the interviewer's perception rather than the applicant's identity.

**Next step:** Lesson 32 closes Part 4 with **mediation**: controlled and natural direct and indirect effects, and the mediation formulas.

### Check Your Understanding

1. In the court case, the defence points to $P(y \mid x) < P(y \mid x')$. State in one sentence what that comparison measures and why it does not answer the legal question.
2. Change the experimental data so that $P(y \mid do(x')) = 0.020$ and recompute the bounds (4.30). Is it still "more probable than not" that the drug caused Mr A's death?
3. Explain why $PNS$ cannot be computed from a perfect randomized experiment without an assumption such as monotonicity. What does the experiment fail to reveal?

---

## Further Reading

*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 4, Sections 4.4.3–4.4.4 and 4.5.1 (Eqs. 4.23–4.42, Table 4.5, Theorem 4.5.1, Figure 4.5, Example 4.5.1); Tian & Pearl (2000); Pearl, *Causality* (2000), Chapter 9.
