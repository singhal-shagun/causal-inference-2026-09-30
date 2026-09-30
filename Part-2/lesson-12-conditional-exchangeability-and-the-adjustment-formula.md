---
type: Lesson
title: "Lesson 12 — Conditional Exchangeability and the Adjustment Formula"
description: "Adjusting for confounders Z restores exchangeability within strata and yields the adjustment formula for the ATE."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: human:shagun-with-ai, at: 2026-07-21 }
---

# Lesson 12: Conditional Exchangeability and the Adjustment Formula

## Where We Left Off

**Lesson 10** showed that raw observational data mixes causation with contamination from confounding bias:

$$E[Y \mid X=1] - E[Y \mid X=0] = \underbrace{E[Y(1) - Y(0)]}_{\text{True ATE}} + \underbrace{\Big( E[Y(0) \mid X=1] - E[Y(0) \mid X=0] \Big)}_{\text{Confounding Bias } B}$$

**Lesson 11** identified the condition that eliminates $B$ — **exchangeability**, $(Y(1), Y(0)) \perp\!\!\!\perp X$ — and showed that a randomized trial delivers it by construction. But it also flagged the uncomfortable reality: in observational data, people choose their own treatment, so unconditional exchangeability almost never holds.

Lesson 11 closed by pointing to the escape route: **conditional exchangeability**, $(Y(1), Y(0)) \perp\!\!\!\perp X \mid Z$. That was a promise, not a procedure. This lesson delivers the procedure.

Specifically, we answer two questions:

1. **What does conditional exchangeability actually buy us?** (It restores comparability *inside* each stratum of $Z$.)
2. **How do we convert that stratum-level comparability into a single population-level number?** (The **adjustment formula**.)

> [!NOTE]
> **A Familiar Destination:** The adjustment formula is not new. We met it in **Lesson 05**, where the *causal graph* told us to block the back-door path $X \leftarrow Z \to Y$ by conditioning on $Z$. Back then it arrived as a graphical prescription. Here we will *derive* it from potential-outcomes algebra — the same formula, now with a proof.

---

## Recovering Comparability One Stratum at a Time

Recall the drug example from Lesson 11: sicker patients preferentially took the drug, so the treated group's baseline ($40$) was far worse than the control group's ($70$). The groups were not exchangeable, and the observed association reversed the sign of the true effect.

But notice *why* the groups differed: they differed **in baseline health**. Suppose we record baseline health as a variable $Z$ with levels $\{\text{severe}, \text{moderate}, \text{mild}\}$. The treated group was over-represented in the severe level and the control group in the mild level — which is precisely what made them non-comparable overall.

Now restrict attention to a single level. Among patients who were *all* severe at baseline, some happened to take the drug and some did not. Within that slice, the two sub-groups are no longer systematically different: they share the same baseline severity, so their baseline potential outcomes $Y(0)$ match.

```text
GLOBAL POPULATION (Confounded)
  Treatment Group (X=1)      vs.   Control Group (X=0)
  [mostly severe patients]         [mostly mild patients]
  E[Y(0) | X=1] = 40               E[Y(0) | X=0] = 70
  --> NOT comparable (Unconditional Exchangeability fails)

WITHIN STRATUM Z = "severe"
  Treatment Group (X=1)      vs.   Control Group (X=0)
  [severe patients]                [severe patients]
  E[Y(0) | X=1, Z=severe] = 40     E[Y(0) | X=0, Z=severe] = 40
  --> Comparable (Conditional Exchangeability holds!)

WITHIN STRATUM Z = "mild"
  Treatment Group (X=1)      vs.   Control Group (X=0)
  [mild patients]                  [mild patients]
  E[Y(0) | X=1, Z=mild] = 70       E[Y(0) | X=0, Z=mild] = 70
  --> Comparable (Conditional Exchangeability holds!)
```

The baseline gap of $40$ vs. $70$ that wrecked the global comparison has disappeared *inside* each stratum. It re-appears only when we pool the strata together, because the treatment and control groups draw from them in different proportions.

This is the central move of the lesson: **we do not need the whole population to be exchangeable. We only need each slice of it to be.**

### Intuition: Many Small Randomized Trials

If conditional exchangeability holds, the observational study behaves like a collection of miniature randomized trials — one per stratum of $Z$. Inside the "severe" stratum, treatment assignment is as good as a coin flip; likewise inside "moderate", "mild", and so on.

Each mini-trial gives an unbiased effect estimate *for that stratum*. The remaining task is bookkeeping: combine the per-stratum effects into one population number, weighting each stratum by how common it is. That weighted recombination **is** the adjustment formula.

---

## Formula & Definition

Conditional Exchangeability is mathematically written as:
$$(Y(1), Y(0)) \perp\!\!\!\perp X \mid Z$$

This states that once we condition on $Z$, the actual treatment assignment $X$ is independent of the potential outcomes. Within each stratum of $Z$, it is as if treatment was randomly assigned.

Equivalently, in the language of Lesson 10's bias term, the confounding bias vanishes **within** every stratum:

$$B_z = E[Y(0) \mid X=1, Z=z] - E[Y(0) \mid X=0, Z=z] = 0 \quad \text{for every } z$$

> [!WARNING]
> **Conditional exchangeability is strictly weaker than unconditional.** Unconditional exchangeability implies its conditional cousin, but not the reverse. This weakening is exactly what makes observational causal inference possible — and exactly what makes it fragile, since it demands that we have measured *every* confounder in $Z$.

---

## Deriving the Adjustment Formula

If we assume Conditional Exchangeability, we can derive the **Adjustment Formula** (first seen graphically in Lesson 05) using probability theory:

$$
\begin{aligned}
P(Y(x) = y) &= \sum_{z} P(Y(x) = y \mid Z=z) P(Z=z) && \text{(Law of Total Probability)} \\
&= \sum_{z} P(Y(x) = y \mid X=x, Z=z) P(Z=z) && \text{(Conditional Exchangeability)} \\
&= \sum_{z} P(Y = y \mid X=x, Z=z) P(Z=z) && \text{(Consistency)}
\end{aligned}
$$

Thus, the causal parameter $P(Y(x) = y)$ (often written as $P(Y|do(X=x))$) can be estimated using purely observational distributions of $Y$, $X$, and $Z$.

<details>
<summary><strong>Formal Definition: Consistency Assumption</strong></summary>

The **Consistency** assumption states that an individual's observed outcome $Y$ is equal to their potential outcome under the treatment value $x$ they actually received:
$$Y = Y(x) \quad \text{if} \quad X=x$$
This rules out hidden variations in treatment (e.g., getting different "dosages" or different quality levels of the same drug) and guarantees that the observed conditional probability maps directly to the counterfactual state.
</details>

> [!NOTE]
> This derivation shows that **Consistency** and **Conditional Exchangeability** are the mathematical requirements that allow us to translate counterfactual parameters into statistical ones.

## Conditional Exchangeability vs. Consistency

In the derivation above, the last two equations may look very similar, but they are conceptually and mathematically distinct!

Look closely at the left-hand side of the conditioning bar:

1. In the first equation, we have: **$Y(x)$** (the *potential outcome* / counterfactual).
2. In the second equation, we have: **$Y$** (the *observed outcome*).

Here is why they represent different steps in the derivation:

### Step 1: Conditional Exchangeability

$$\sum_{z} P(\mathbf{Y(x)} = y \mid Z=z) P(Z=z) \to \sum_{z} P(\mathbf{Y(x)} = y \mid X=x, Z=z) P(Z=z)$$

* **What this does:** By assuming conditional exchangeability, we are allowed to add the event "$X=x$" to the conditional side of the probability.
* **Intuition:** It says, "Among people with the same $Z$, their potential outcomes $Y(x)$ are independent of whether they actually got treatment $X=x$. So we can write $P(Y(x) | Z)$ as $P(Y(x) | X=x, Z)$."

### Step 2: Consistency

$$\sum_{z} P(\mathbf{Y(x)} = y \mid X=x, Z=z) P(Z=z) \to \sum_{z} P(\mathbf{Y} = y \mid X=x, Z=z) P(Z=z)$$

* **What this does:** We replace the counterfactual variable **$Y(x)$** with the actually observed variable **$Y$**.
* **Why we can do it:** The consistency rule says: *If you received treatment $X=x$, then your observed outcome $Y$ is equal to your potential outcome $Y(x)$*. Since we are already conditioning on $X=x$ inside the probability, we can swap $Y(x)$ for the observed $Y$.

### Summary

The first step introduces the treatment assignment $X=x$ to the *conditioning* side using **Exchangeability**. Once $X=x$ is present on the right side, the second step translates the *potential outcome* variable $Y(x)$ into the *observed outcome* variable $Y$ using **Consistency**.

Without both steps, we cannot link unobserved potential outcomes to the observed data!

---

## A Numerical Walkthrough: Healing the Lesson 11 Sign Reversal

Let us put the adjustment formula to work on the exact drug study from **Lesson 11 Scenario A**, where confounding reversed the true causal effect:

* **True causal effect:** The drug adds $+20$ points to health for every patient ($Y(1) = Y(0) + 20$).
* **Population mix ($Z$):** 50% of patients have severe baseline illness ($P(Z=\text{severe}) = 0.5$), and 50% have mild illness ($P(Z=\text{mild}) = 0.5$).
* **The confounding problem:** Sicker patients preferentially took the drug — 45 of 50 severe patients versus 5 of 50 mild ones. Raw observation showed a difference of **$-4$**, making the drug look harmful.
* **Why adjustment is available:** because the preference was not absolute, every stratum contains both treated and control patients. Lesson 13 names this requirement **positivity**; without it the table below could not be filled in.

### 1. Stratum-Specific Observational Means

Within each stratum of baseline health $Z$, conditional exchangeability holds:

| Stratum ($Z$) | Proportion $P(Z=z)$ | Treated Mean: $E[Y \mid X=1, Z=z]$ | Control Mean: $E[Y \mid X=0, Z=z]$ | Stratum Effect |
| :--- | :---: | :---: | :---: | :---: |
| **Severe** | $0.5$ | $40 + 20 = \mathbf{60}$ *(45 patients)* | $\mathbf{40}$ *(5 patients)* | $60 - 40 = \mathbf{+20}$ |
| **Mild** | $0.5$ | $70 + 20 = \mathbf{90}$ *(5 patients)* | $\mathbf{70}$ *(45 patients)* | $90 - 70 = \mathbf{+20}$ |

Inside every stratum, the drug effect is $+20$.

### 2. Applying the Adjustment Formula

Now we compute the population expected outcomes under each intervention:

**Step A — Counterfactual average if everyone were treated ($do(X=1)$):**

$$
\begin{aligned}
E[Y \mid do(X=1)] &= \sum_{z} E[Y \mid X=1, Z=z] P(Z=z) \\
&= \Big( E[Y \mid X=1, Z=\text{severe}] \times P(Z=\text{severe}) \Big) + \Big( E[Y \mid X=1, Z=\text{mild}] \times P(Z=\text{mild}) \Big) \\
&= (60 \times 0.5) + (90 \times 0.5) \\
&= 30 + 45 = \mathbf{75}
\end{aligned}
$$

**Step B — Counterfactual average if everyone were control ($do(X=0)$):**

$$
\begin{aligned}
E[Y \mid do(X=0)] &= \sum_{z} E[Y \mid X=0, Z=z] P(Z=z) \\
&= \Big( E[Y \mid X=0, Z=\text{severe}] \times P(Z=\text{severe}) \Big) + \Big( E[Y \mid X=0, Z=\text{mild}] \times P(Z=\text{mild}) \Big) \\
&= (40 \times 0.5) + (70 \times 0.5) \\
&= 20 + 35 = \mathbf{55}
\end{aligned}
$$

### 3. The Recovered Causal Effect (ATE)

$$\text{ATE} = E[Y \mid do(X=1)] - E[Y \mid do(X=0)] = 75 - 55 = \mathbf{+20}$$

The raw unadjusted comparison gave **$-4$** (confounded by baseline illness). The adjustment formula re-weights the strata to reflect the true population proportions, completely eliminating the confounding bias and recovering the true causal uplift of **$+20$ points**.

---

## Closing the Loop: Graph and Algebra Agree

We have now reached the same destination by two independent routes.

| | **Part 1 Route (Lesson 05)** | **Part 2 Route (this lesson)** |
| :--- | :--- | :--- |
| **Starting point** | A causal DAG with $X \leftarrow Z \to Y$ | Potential outcomes $(Y(1), Y(0))$ |
| **Key condition** | $Z$ satisfies the back-door criterion | $(Y(1), Y(0)) \perp\!\!\!\perp X \mid Z$ |
| **Justification** | Conditioning on $Z$ blocks the back-door path | Within each stratum, assignment is as-if random |
| **Result** | $\sum_z P(Y \mid X{=}x, Z{=}z)\,P(Z{=}z)$ | $\sum_z P(Y \mid X{=}x, Z{=}z)\,P(Z{=}z)$ |

> [!IMPORTANT]
> **Two Languages, One Formula.** The back-door criterion and conditional exchangeability are not competing ideas — they are the graphical and algebraic statements of the same requirement. Pearl's DAGs tell you *which* variables belong in $Z$; the potential-outcomes derivation tells you *what to compute* once you have them.

---

## Summary and Key Takeaways

1. **The Weakening:** Observational data rarely satisfies unconditional exchangeability, so we settle for the weaker conditional exchangeability $(Y(1), Y(0)) \perp\!\!\!\perp X \mid Z$.
2. **The Mechanism:** Conditional exchangeability makes each stratum of $Z$ behave like its own miniature randomized trial ($B_z = 0$ for every $z$).
3. **The Recombination:** The adjustment formula reassembles stratum-level effects into a population estimate, weighting by $P(Z=z)$.
4. **Two Assumptions, Not One:** The derivation needs *both* conditional exchangeability (to insert $X=x$ into the conditioning set) *and* consistency (to swap $Y(x)$ for the observed $Y$).
5. **Graph ≡ Algebra:** The result is identical to the back-door adjustment of Lesson 05, reached from a completely different starting point.
6. **Next Step:** The formula requires $P(Y \mid X{=}x, Z{=}z)$ to exist for *every* stratum. Lesson 13 examines what happens when some stratum contains no treated (or no untreated) units — the **positivity** assumption.

---

### Further Reading

*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 4.
