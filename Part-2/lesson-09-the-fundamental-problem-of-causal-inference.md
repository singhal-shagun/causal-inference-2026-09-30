---
type: Lesson
title: "Lesson 09 — The Fundamental Problem and the ATE"
description: "Why the Individual Causal Effect is unobservable, and how the Average Treatment Effect makes causal inference possible."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: human:shagun-with-ai, at: 2026-07-21 }
---

# Lesson 09: The Fundamental Problem and the ATE

## The Core Dilemma

As we defined in Lesson 08, the **Individual Causal Effect ($\text{ICE}_u$)** for unit $u$ is:

$$\text{ICE}_u = Y_u(1) - Y_u(0)$$

However, for any single unit $u$, we can only ever observe one of the potential outcomes: $Y_u(1)$ (if treated) OR $Y_u(0)$ (if control).

This unobservability of the counterfactual state is known as **The Fundamental Problem of Causal Inference**.

## Why It Matters

Because we cannot observe both potential outcomes simultaneously for the same individual, we cannot calculate individual treatment effects directly from data. We are always missing half of the potential outcome data (the counterfactuals).

## Moving to the Population Level: The ATE

Since we cannot calculate $\text{ICE}_u$ for individuals, we pivot to estimating the **Average Causal Effect (ACE)** across a population, more commonly known in literature as the **Average Treatment Effect (ATE)**.

The ATE represents the expected difference in outcomes if the *entire* population were treated versus if the *entire* population were left as control.

Mathematically, the ATE is the expectation of the individual causal effects across the population:

$$\text{ATE} = E[Y(1) - Y(0)]$$

By the **linearity of expectation**, this decomposes into:

$$\text{ATE} = E[Y(1)] - E[Y(0)]$$

### Intuition: Linearity of Expectation as a Causal Bridge

Linearity of expectation ($E[A - B] = E[A] - E[B]$) performs a critical "bridge" in causal inference:

* **The Unobservable Joint Query ($E[Y(1) - Y(0)]$):** Would require observing both $Y_u(1)$ and $Y_u(0)$ for every individual $u$, which is blocked by the Fundamental Problem.
* **The Marginal Expectations ($E[Y(1)] - E[Y(0)]$):** Splits the problem into two separate population averages—the average outcome if the *entire population* were treated, minus the average outcome if the *entire population* were control.

Thanks to linearity of expectation, we do not need to observe counterfactual pairs for any single individual; we only need to estimate two population-level marginal averages.

> [!NOTE]
> **From Individuals to Populations:**
>
> * **ICE ($\text{ICE}_u = Y_u(1) - Y_u(0)$):** Impossible to calculate directly due to unobservable counterfactuals.
> * **ATE ($\text{ATE} = E[Y(1)] - E[Y(0)]$):** Estimable from sample data if we satisfy key identification assumptions (such as exchangeability).

### A Note on Notation: Expectations and Probabilities

Two notations for this same quantity appear across the lessons that follow, and it is worth reconciling them now rather than meeting them as an apparent contradiction later.

The expectation form above is the **general** one. $E[Y(1)] - E[Y(0)]$ is meaningful whatever $Y$ measures — blood pressure, days in hospital, income. But when $Y$ is **binary**, coded $1$ for the event of interest and $0$ otherwise, an expectation is a probability:

$$E[Y] = 1 \cdot P(Y=1) + 0 \cdot P(Y=0) = P(Y=1)$$

So for a binary outcome $E[Y(1)] = P(Y(1)=1)$, and the ATE may be written either way:

$$\text{ATE} = E[Y(1)] - E[Y(0)] = P(Y(1)=1) - P(Y(0)=1)$$

These are not two definitions but one, expressed in two notations. Pearl's *Primer* works with distributions rather than moments, stating causal quantities as $P(Y=y \mid do(x))$, so the probability form is what appears in the book's adjustment formula and in the worked example of **Lesson 15**; the expectation form is what appears wherever the outcome is left unrestricted, as in **Lesson 13**. When the outcome is binary, a difference of probabilities of this kind is also called the **risk difference**.

> [!TIP]
> If a formula in a later lesson seems to have switched notation without warning, check whether the outcome is binary. If it is, $E$ and $P$ are interchangeable and nothing has changed.

## From Potential Outcomes to Observed Data

While we cannot calculate $Y_u(1) - Y_u(0)$ for a single person, we can estimate $E[Y(1)]$ and $E[Y(0)]$ using observed group averages:

* **Group Average (Treated Group):** We observe the average outcome for people who actually received treatment: $E[Y \mid X=1]$.
* **Group Average (Control Group):** We observe the average outcome for people who were actually in control: $E[Y \mid X=0]$.

> [!IMPORTANT]
> **The Critical Assumption:** For $E[Y \mid X=1] = E[Y(1)]$ to hold, we need **Exchangeability (Ignorability)**. Without this, the average of the treated group is just a biased reflection of the potential outcomes, and the difference $E[Y \mid X=1] - E[Y \mid X=0]$ will be confounded by $Z$.

## Roadmap of Identification Assumptions

To estimate the population parameters $E[Y(1)]$ and $E[Y(0)]$ from observed sample statistics ($E[Y \mid X=1]$ and $E[Y \mid X=0]$), we must satisfy three core structural assumptions (which will be formalized in Lessons 12–15):

1. **Exchangeability (Ignorability)** *(Formalized in Lessons 12 & 13)*: Treatment assignment $X$ is independent of potential outcomes $(Y(1), Y(0))$.
   * This is the Potential Outcomes equivalent of "having no unblocked back-door paths in the DAG."
2. **Positivity** *(Formalized in Lesson 14)*: Every individual in the population has a non-zero probability of receiving either treatment level ($0 < P(X=1 \mid Z) < 1$).
3. **Consistency** *(Formalized in Lessons 13 & 15)*: The observed outcome $Y$ under actual treatment assignment $X=x$ equals the potential outcome $Y(x)$.

> [!NOTE]
> While the *formal names and potential-outcome math* of Exchangeability, Positivity, and Consistency are introduced in Lessons 12–15, their **intuitive DAG equivalents** were covered in Part 1 (Lessons 01–05):
>
> * **Exchangeability** is the Potential Outcomes equivalent of *"having no unblocked back-door paths in the DAG."*
> * **Positivity** is the practical requirement that every stratum $Z=z$ contains both treated ($X=1$) and control ($X=0$) units so we don't divide by zero in the adjustment formula (Lesson 05).
> * **Consistency** is the assumption that the observed $Y$ when $X=x$ matches the counterfactual $Y(x)$.

---

### Further Reading

*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 4.
