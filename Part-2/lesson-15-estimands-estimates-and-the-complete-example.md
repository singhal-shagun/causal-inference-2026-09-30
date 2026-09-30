---
type: Lesson
title: "Lesson 15 — Estimands, Estimates, and the Complete Example"
description: "The four objects of any causal analysis — causal estimand, statistical estimand, estimator, estimate — and the identification and estimation steps that join them, worked through a complete example."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: human:shagun-with-ai, at: 2026-07-22 }
---

# Lesson 15: Estimands, Estimates, and the Complete Example

## Putting It Together: The Key Terms

Now that we have SUTVA, Exchangeability, and Positivity under our belt, we can formally outline the steps of any causal analysis. There are **four** distinct objects, joined by **two** distinct activities, and keeping them apart is what prevents the whole enterprise from looking like a single unexplained leap:

1. **Causal Estimand**: The theoretical target we want to learn, written in terms of potential outcomes. E.g., the Average Treatment Effect $\text{ATE} = E[Y(1)] - E[Y(0)]$. It is the true, population-level counterfactual difference that we *wish* we could observe, and it is not a function of the observed data.

    *Then comes* ***identification***, *the derivation carried out on paper, licensed by the four assumptions of Lesson 14.*

2. **Statistical Estimand**: The same quantity rewritten so that every term is a functional of the observed distribution. E.g., the Adjustment Formula:
   $$P(Y(x)=1) = \sum_{z} P(Y=1 \mid X=x, Z=z)\, P(Z=z)$$
   Note what this is and is not. It is still a **population-level** quantity — it is written with true probabilities, not sample frequencies — but it no longer mentions a counterfactual. Identification has done its work the moment this line can be written down.

    *Then comes* ***estimation***, *the statistical activity of approximating a population quantity from a finite sample.*

3. **Estimator**: The recipe applied to the *sample*: the sample analogue of the statistical estimand, with observed frequencies in place of the true probabilities.
   $$\widehat{P}(Y(x)=1) = \sum_{z} \widehat{P}(Y=1 \mid X=x, Z=z)\, \widehat{P}(Z=z)$$
   Different estimators can target the same statistical estimand — stratification, matching, and inverse probability weighting are three, each with different variance behaviour.
4. **Estimate**: The actual **numerical value** the estimator yields on the data in hand. E.g., $\text{ATE} = 0.18$ (18 percentage points).

> [!IMPORTANT]
> **Identification and estimation answer different questions.** Identification asks *whether the causal quantity can be written in terms of observables at all* — a question about the population and the assumptions, settled algebraically before any data is collected. Estimation asks *how accurately a finite sample pins down that expression*. A quantity can be perfectly identified and badly estimated: the practical-positivity case of Lesson 13, where a propensity of $0.99$ leaves the statistical estimand perfectly well-defined and the estimate worthless. No amount of data repairs a failure of identification, and no amount of clever identification repairs a sample of ten.

> [!CAUTION]
> **Causal Assignment vs. Potential Outcome:**
>
> * $X$ (Treatment) is a physical, observed assignment variable (e.g., $X=1$ or $X=0$). You can look it up in the dataset.
> * By contrast, $Y(1)$ and $Y(0)$ are potential outcomes, and the $\text{ATE}$ ($E[Y(1)] - E[Y(0)]$) is the population-level counterfactual quantity — the **causal estimand** — that we seek to identify.
> * The assignment value $X$ is **not** a counterfactual. It sits beside $Y(1)$ in the same formulas, which makes it easy to mistake for one.

### Graphical Summary of the Mapping

```text
   CAUSAL ESTIMAND          STATISTICAL ESTIMAND            ESTIMATOR              ESTIMATE
 +-----------------+       +--------------------+      +----------------+      +------------+
 |E[Y(1)] - E[Y(0)]|       | Sum_z              |      | Same formula,  |      |    0.18    |
 |                 |       |  P(Y=1|X=x,Z=z)    |      | sample freqs   |      |            |
 | counterfactual  |       |  * P(Z=z)          |      | in place of    |      | one number,|
 | population      |       | observational,     |      | probabilities  |      | this data  |
 | quantity        |       | population quantity|      | (a rule)       |      |            |
 +-----------------+       +--------------------+      +----------------+      +------------+
          |                          |                         |                      |
          +---- IDENTIFICATION ------+---- ESTIMATION ---------+----------------------+
            (assumptions, on paper)     (statistics, on the sample)
```

The left arrow is where the four assumptions of Lesson 14 are spent. The right arrow is where sample size and variance live. Confusing them is what makes causal inference look like statistics with extra vocabulary — it is not, and the first arrow is the part statistics alone cannot supply.

---

## The Complete Causal Workflow

To calculate $P(Y(x) = y)$ in a non-experimental setting:

```text
  [ Causal Assumption ]
  No interference, Consistency, Conditional Exchangeability, Positivity
                          |
                          v
        [ Causal Estimand ]  -->  P(Y(x) = y)
                          |
                          |  IDENTIFICATION (uses the assumptions above)
                          v
      [ Statistical Estimand ]  -->  Sum_z P(Y=y | X=x, Z=z) P(Z=z)
                          |
                          |  ESTIMATION (uses the sample)
                          v
         [ Estimator -> Estimate ]  -->  Empirical value computed from data
```

---

## Numerical Step-by-Step Example

Consider a hypothetical dataset of 1,000 sicker/elderly ($Z=1$) and healthier/young ($Z=0$) patients where we evaluate the influence of a heart medication ($X$) on patients' cardiac recovery ($Y$).

### 1. The Observational Data (Raw Counts)

From our population of **1,000 patients**, we collect the following observational frequencies:

* **Elderly Patients ($Z=1$) ($400$ total patients):**
  * $P(Z=1) = \frac{400}{1000} = 0.4$
  * **Treated ($X=1$):** 200 patients, of which 100 recovered ($Y=1$).
    * $P(Y=1 \mid X=1, Z=1) = \frac{100}{200} = 0.5$
  * **Untreated ($X=0$):** 200 patients, of which 40 recovered ($Y=1$).
    * $P(Y=1 \mid X=0, Z=1) = \frac{40}{200} = 0.2$

* **Young Patients ($Z=0$) ($600$ total patients):**
  * $P(Z=0) = \frac{600}{1000} = 0.6$
  * **Treated ($X=1$):** 500 patients, of which 450 recovered ($Y=1$).
    * $P(Y=1 \mid X=1, Z=0) = \frac{450}{500} = 0.9$
  * **Untreated ($X=0$):** 100 patients, of which 80 recovered ($Y=1$).
    * $P(Y=1 \mid X=0, Z=0) = \frac{80}{100} = 0.8$

### 2. Verifying the Identifiability Assumptions

Before executing any calculations, we must verify that our dataset satisfies the prerequisites for causal identification:

1. **SUTVA (Stable Unit Treatment Value Assumption)**:
   * *No Interference*: We assume one patient's recovery status ($Y$) is independent of another patient's treatment assignment ($X$) (i.e., cardiac recovery is not contagious; there are no herd-immunity or herd-exposure effects).
   * *Consistency*: The treatment assignment $X=1$ represents a single, uniform dosage and standard of medication, ruling out hidden variations in treatment.
2. **Conditional Exchangeability**:
   * Our background structure asserts that Age ($Z$) is the only common cause of Treatment ($X$) and Recovery ($Y$). By conditioning on $Z$, we block the back-door path $X \leftarrow Z \to Y$. Within each stratum of $Z$, treatment assignment is independent of potential outcomes.
3. **Positivity**:
   * We calculate the probability of being treated in each subgroup of $Z$:
     * Elderly ($Z=1$): $P(X=1 \mid Z=1) = \frac{200}{400} = 0.5$
     * Young ($Z=0$): $P(X=1 \mid Z=0) = \frac{500}{600} \approx 0.83$
   * Both values lie strictly between $0$ and $1$, meaning every patient has a non-zero chance of receiving either treatment level.

Since all assumptions are satisfied, estimation of the ATE is mathematically valid.

### 3. Computing the ATE using Adjustment

Using the adjustment formula:
$$P(Y(x)=1) = \sum_{z} P(Y=1 \mid X=x, Z=z) P(Z=z)$$

Note the switch from the expectation form in which the estimand was stated above. Recovery is a **binary** outcome here, so by the identity of Lesson 09, $E[Y(x)] = P(Y(x)=1)$: the probability form below computes precisely the ATE named as the estimand, in the notation the *Primer* uses.

* **Under Treatment ($X=1$):**

    $$
    \begin{aligned}
    P(Y(1)=1) &= \sum_{z} P(Y=1 \mid X=1, Z=z)\, P(Z=z) \\
    &= P(Y=1 \mid X=1, Z=1)\, P(Z=1) + P(Y=1 \mid X=1, Z=0)\, P(Z=0) \\
    &= (0.5 \times 0.4) + (0.9 \times 0.6) \\
    &= 0.2 + 0.54 \\
    &= \mathbf{0.74}
    \end{aligned}
    $$

* **Under Control ($X=0$):**

    $$
    \begin{aligned}
    P(Y(0)=1) &= \sum_{z} P(Y=1 \mid X=0, Z=z)\, P(Z=z) \\
    &= P(Y=1 \mid X=0, Z=1)\, P(Z=1) + P(Y=1 \mid X=0, Z=0)\, P(Z=0) \\
    &= (0.2 \times 0.4) + (0.8 \times 0.6) \\
    &= 0.08 + 0.48 \\
    &= \mathbf{0.56}
    \end{aligned}
    $$

### 4. Causal Effect

$$\text{ATE} = P(Y(1)=1) - P(Y(0)=1) = 0.74 - 0.56 = 0.18$$

The treatment causes an average increase of **18 percentage points** in the probability of recovery.

> [!NOTE]
> Notice that without adjusting for $Z$, calculating the raw association $P(Y=1 \mid X=1) - P(Y=1 \mid X=0)$ would yield:
>
> * $P(Y=1 \mid X=1) = \frac{100+450}{200+500} = \frac{550}{700} \approx 0.785$
> * $P(Y=1 \mid X=0) = \frac{40+80}{200+100} = \frac{120}{300} = 0.40$
> * Raw Association: $0.785 - 0.40 = 0.385$ (38.5 percentage points)
>
> The raw association is significantly biased upward because sicker elderly patients ($Z=1$) were less likely to be given the treatment by doctors ($X=1$). Adjusting for $Z$ blocks this confounding bias.

---

### Further Reading

*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 4.
