---
type: Lesson
title: "Lesson 14 — No Interference, Consistency, and the Full Assumption Set"
description: "SUTVA's two requirements — no interference and consistency — completing the assumption set for causal identification."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: human:shagun-with-ai, at: 2026-07-22 }
---

# Lesson 14: No Interference, Consistency, and the Full Assumption Set

## Where We Left Off

Lessons 12 and 13 dealt with the two assumptions that govern *strata*. Exchangeability makes the within-stratum contrast **correct**; positivity makes it **computable**. Both of them take the potential outcomes $Y(1)$ and $Y(0)$ as given, and ask only what we are entitled to do with them.

This lesson steps back and asks whether that notation was well-posed to begin with. Writing $Y_u(x)$ — "the outcome unit $u$ would have had under treatment $x$" — presumes that such a thing is a single, definite number. Two separate conditions are required for that presumption to hold, and neither is automatic.

---

## The SUTVA Assumptions

The two conditions are collected under the **Stable Unit Treatment Value Assumption (SUTVA)**. The name is descriptive: it is the assumption that the *value* of a *treatment* for a *unit* is **stable** — one treatment label, one unit, one outcome.

$Y_u(x)$ can fail to name a single number in two distinct ways:

1. **No Interference** — the outcome may depend on what treatment *other* units received.
2. **Consistency** — the label $x$ may not pick out a single, well-defined intervention.

Each requirement blocks one of these failures.

---

## 1. No Interference

No Interference states that the potential outcome for any individual unit $u$ depends only on the treatment assigned to $u$, and is unaffected by the treatment assigned to any other unit $i \neq u$.

### The Contravention: Spillover Effects

If a vaccine program protects an unvaccinated person because all their neighbors got vaccinated, we have a **spillover effect** (or herd immunity). This violates the "No Interference" assumption because the potential outcome $Y_u(0)$ (remaining infection-free) depends on other people's treatment status.

Notice what this does to the notation. In full generality a unit's outcome is a function of the *entire* assignment vector across the study:

$$Y_u(x_1, x_2, \ldots, x_n)$$

With $n$ units and a binary treatment, that is $2^n$ potential outcomes per unit rather than two. The individual effect $Y_u(1) - Y_u(0)$ is not merely hard to estimate under interference — it is not defined, because there is no single $Y_u(1)$ to subtract from. No Interference is the assumption that collapses this vector to its $u$-th component:

$$Y_u(x_1, \ldots, x_n) = Y_u(x_u)$$

Every lesson since Lesson 08 has written $Y_u(1)$ and $Y_u(0)$ without comment. This is the assumption that licensed the notation.

---

## 2. Consistency (Single Version of Treatment)

As introduced in Lesson 12, consistency requires that the observed outcome $Y_u$ maps directly to the potential outcome under treatment $x$ actually received:
$$Y_u = Y_u(x) \quad \text{if} \quad X_u=x$$

For consistency to hold, there must be **no hidden versions of treatment**. If $X=1$ represents "taking a drug," but some patients take a high-quality brand-name version while others take a degraded generic version, then the treatment $X=1$ is not consistent across units.

---

## The Full Identifiability Assumption Set

With SUTVA defined, we have the **four assumptions** required to identify a causal effect from observational data:

| Assumption | What it guarantees | Formal statement |
| :--- | :--- | :--- |
| **No Interference** | Unit $u$'s outcome depends only on $u$'s own treatment | $Y_u(x_1, \ldots, x_n) = Y_u(x_u)$ |
| **Consistency** | The label $x$ names a single, well-defined intervention | $Y_u = Y_u(x)$ whenever $X_u = x$ |
| **Exchangeability / Conditional Exchangeability** | Treated and control are comparable within each stratum | $(Y(1), Y(0)) \perp\!\!\!\perp X \mid Z$ |
| **Positivity** | Every stratum contains both arms | $0 < P(X=1 \mid Z=z) < 1$ |

The first two are SUTVA and concern the *estimand*: whether the quantity we are chasing is well-defined at all. The last two concern the *data*: whether what we have observed can reach that quantity. This is why all four are needed and why none can substitute for another.

### Where Each One Enters the Derivation

Lesson 12 derived the adjustment formula in three steps. Each assumption licenses a specific move, and it is worth seeing them attached to the line they justify:

$$
\begin{aligned}
P(Y(x) = y) &= \sum_{z} P(Y(x) = y \mid Z=z)\, P(Z=z) && \text{(Law of Total Probability)} \\
&= \sum_{z} P(Y(x) = y \mid X=x, Z=z)\, P(Z=z) && \text{(Exchangeability)} \\
&= \sum_{z} P(Y = y \mid X=x, Z=z)\, P(Z=z) && \text{(Consistency)}
\end{aligned}
$$

**No Interference** acts before the first line is written: it is what allows $Y(x)$ to appear at all, rather than $Y(x_1, \ldots, x_n)$. **Exchangeability** licenses inserting $X=x$ into the conditioning set. **Consistency** licenses replacing the counterfactual $Y(x)$ with the observed $Y$. **Positivity** does not license a line — it guarantees that the expression on the final line is defined for every $z$ in the sum.

### The Four Failure Modes Are Not the Same Failure

A common shorthand holds that violating any assumption "biases the estimate." That is true of only one of them, and the distinction matters when diagnosing a study:

| Assumption violated | What actually goes wrong | Symptom |
| :--- | :--- | :--- |
| **No Interference** | The estimand is ill-posed; $Y_u(1) - Y_u(0)$ names nothing | There is no true value to estimate |
| **Consistency** | The observed $Y$ is some other potential outcome | You estimate a well-defined effect of the wrong treatment |
| **Exchangeability** | $B \neq 0$; the groups were not comparable | A **wrong number**, possibly sign-reversed |
| **Positivity** | A conditional mean is an average over zero units | **No number at all** — the formula is undefined |

> [!IMPORTANT]
> Only exchangeability failure produces the classic picture of a biased estimate. A positivity failure does not give a biased answer; as Lesson 13 established, it gives no answer, and any number reported in its place is an extrapolation from the model rather than a measurement from the data. A consistency failure gives an unbiased estimate of a question nobody asked. A no-interference failure means there was no well-defined target in the first place.

---

## What Identifiability Actually Means

These four assumptions are called the **identifiability** assumptions, so it is worth stating precisely what they buy.

Divide the quantities we can write into two kinds:

* A **causal quantity** is one expressed with potential outcomes or the do-operator — $E[Y(1)]$, $P(Y(x)=y)$, $E[Y(1) - Y(0) \mid Z=z]$. No amount of data collection observes one directly, because each refers to outcomes under a treatment some units did not receive.
* A **statistical quantity** is any functional of the observed joint distribution of $(X, Y, Z)$ — $E[Y \mid X=1]$, $P(Z=z)$, $\sum_z E[Y \mid X=1, Z=z] P(Z=z)$. Enough data estimates one to arbitrary precision.

> [!IMPORTANT]
> **Identifiability.** A causal quantity is **identifiable** if it can be computed from purely statistical quantities. **Identification** is the activity of producing that expression — a derivation carried out on paper, before any data is touched.

The adjustment formula is an identification result in exactly this sense. Its left-hand side is causal and unobservable; its right-hand side is statistical and estimable; and the four assumptions are what permit the equals sign between them.

### What Does Not Distinguish Them

It is tempting to read the distinction off the notation — causal quantities look unconditional, statistical ones look conditional. The canonical comparison invites this, since $E[Y(1)]$ carries no conditioning bar and $E[Y \mid X=1]$ does. The reading is wrong, and all four cells are occupied:

| | Statistical | Causal |
| :--- | :--- | :--- |
| **Unconditional** | $E[Y]$ | $E[Y(1)]$ |
| **Conditional** | $E[Y \mid X=1]$ | $E[Y(1) \mid Z=z]$ |

What makes a quantity causal is that it refers to potential outcomes; what makes it statistical is that it is a functional of the observed distribution. Conditioning is orthogonal to both. The covariate-specific effect $E[Y(1) - Y(0) \mid Z=z]$ is conditional and causal, and the right-hand side of the adjustment formula is an average of conditionals and purely statistical.

> [!NOTE]
> **Identification is not estimation.** Identification asks whether the causal quantity can be written in terms of observables *at all* — a question about the population and the assumptions, answered algebraically. Estimation asks how to compute that expression from a finite sample, and how much it will wobble. A quantity can be perfectly identified and badly estimated: this is the practical-positivity case from Lesson 13, where a propensity of $0.99$ leaves the estimand identified and the estimate worthless. Lesson 15 takes up the second half of this split.

---

## Summary and Key Takeaways

1. **SUTVA Licenses the Notation:** Writing $Y_u(x)$ presumes the value is a single definite number. No Interference and Consistency are the two conditions that make it so.
2. **No Interference:** Without it a unit's outcome is $Y_u(x_1, \ldots, x_n)$, one value per assignment of the *whole study*, and the individual effect $Y_u(1) - Y_u(0)$ is undefined rather than merely unobservable.
3. **Consistency:** The treatment label must name one intervention. Hidden versions — brand versus generic, 10 mg versus 50 mg — break the link between the observed $Y$ and the intended $Y(x)$.
4. **Four Assumptions, Two Jobs:** SUTVA makes the *estimand* well-defined; exchangeability and positivity make the *data* capable of reaching it.
5. **The Failures Differ:** Only an exchangeability violation yields a biased number. Positivity yields no number, consistency yields the right answer to the wrong question, and interference means there was no target at all.
6. **Identifiability:** A causal quantity is identifiable when it can be computed from purely statistical quantities. Identification is a derivation, not a computation, and it is complete before any data arrives.
7. **Not the Conditioning Bar:** Causal quantities are not "the unconditional ones." $E[Y(1) - Y(0) \mid Z=z]$ is conditional and causal; the adjustment formula's right-hand side is an average of conditionals and purely statistical.
8. **Next Step:** Lesson 15 follows the chain past identification — from the causal estimand to the statistical estimand, the estimator, and the final estimate.

---

### Further Reading

*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 4.
