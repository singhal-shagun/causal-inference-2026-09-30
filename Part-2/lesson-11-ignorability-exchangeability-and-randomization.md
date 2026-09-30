---
type: Lesson
title: "Lesson 11 — Ignorability, Exchangeability, and Randomization"
description: "The conditions under which confounding bias vanishes, and why randomization guarantees exchangeability."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: human:shagun-with-ai, at: 2026-07-21 }
---

# Lesson 11: Ignorability, Exchangeability, and Randomization

## Bridging from Lesson 10: Eliminating the Confounding Bias

In **Lesson 10**, we proved the fundamental equation of observational bias:

$$\text{Observational Difference} = \text{Causal Effect (ATE)} + \text{Confounding Bias } (B)$$

$$E[Y \mid X=1] - E[Y \mid X=0] = E[Y(1) - Y(0)] + \underbrace{\Big( E[Y(0) \mid X=1] - E[Y(0) \mid X=0] \Big)}_{\text{Confounding Bias } B}$$

We saw that whenever treatment choice ($X$) is confounded by baseline variables ($Z$), the bias term $B \neq 0$, destroying our ability to measure causation from raw data.

This brings us to the core question of this lesson: **Under what formal condition does the confounding bias term $B$ vanish to zero, allowing Observational Association to equal True Causation?**

The answer is **Exchangeability** (also referred to in statistical literature as **Ignorability** or **Unconfoundedness**).

---

## What is Exchangeability?

Intuitive definition: **Exchangeability** means that the treated group ($X=1$) and control group ($X=0$) are fundamentally identical in their underlying characteristics and baseline potential outcomes. If we were to swap or "exchange" the treatment assignments of the two groups, their average potential outcomes would remain exactly the same.

Formally, exchangeability states that potential outcomes $(Y(1), Y(0))$ are statistically independent of actual treatment assignment $X$:

$$(Y(1), Y(0)) \perp\!\!\!\perp X$$

### What Does $(Y(1), Y(0)) \perp\!\!\!\perp X$ Mean Mathematically?

This independence means that knowing whether a person received treatment ($X=1$) or control ($X=0$) provides **zero information** about what their potential outcomes $(Y(1), Y(0))$ would be:

1. **Baseline Potential Outcome Independence:**
   $$E[Y(0) \mid X=1] = E[Y(0) \mid X=0] = E[Y(0)]$$
   *The treated group's baseline counterfactual ($Y(0)$) is identical to the control group's baseline outcome.*

2. **Treatment Potential Outcome Independence:**
   $$E[Y(1) \mid X=1] = E[Y(1) \mid X=0] = E[Y(1)]$$
   *The control group's counterfactual under treatment ($Y(1)$) is identical to the treated group's outcome under treatment.*

---

## Why Exchangeability Drives Confounding Bias $B$ to Zero

Now let's directly plug the consequences of exchangeability back into the **Bias Equation** from Lesson 10. Recall the bias term:

$$B = E[Y(0) \mid X=1] - E[Y(0) \mid X=0]$$

**Step 1 — Apply exchangeability to the treated group's baseline:**

$$E[Y(0) \mid X=1] = E[Y(0)]$$

**Step 2 — Apply exchangeability to the control group's baseline:**

$$E[Y(0) \mid X=0] = E[Y(0)]$$

**Step 3 — Substitute both back into $B$:**

$$B = E[Y(0)] - E[Y(0)] = 0$$

**Step 4 — Collapse the observational difference:**

$$E[Y \mid X=1] - E[Y \mid X=0] = \underbrace{E[Y(1) - Y(0)]}_{\text{True ATE}} + \underbrace{0}_{B} = \text{ATE}$$

$$\boxed{\text{Association } (E[Y \mid X=1] - E[Y \mid X=0]) = \text{Causation } (E[Y(1)] - E[Y(0)])}$$

> [!IMPORTANT]
> **The Key Link to Lesson 10:**
> Exchangeability is the mathematical condition that sets the confounding bias term $B = 0$. Without exchangeability, $B \neq 0$, and observational group differences remain contaminated by baseline differences that have nothing to do with treatment.

One subtlety deserves mention before we move on.

> [!NOTE]
> **A Hidden Co-Assumption — Consistency:**
> The step $E[Y \mid X=1] = E[Y(1) \mid X=1]$ silently relies on **Consistency**: the observed outcome $Y$ for a unit assigned $X=x$ *is* that unit's potential outcome $Y(x)$. Exchangeability alone is not enough; we formalize consistency in Lesson 15.

---

## A Worked Numeric Example: Exchangeable vs. Non-Exchangeable

Consider a clinical trial of $N = 100$ patients evaluating a new drug ($X$) on health recovery score ($Y$).

* **Population composition ($Z$):** 50 patients have severe baseline illness ($Z = \text{severe}$) with an expected baseline score without drug of $Y(0) = 40$. The other 50 patients have mild baseline illness ($Z = \text{mild}$) with an expected baseline score without drug of $Y(0) = 70$.
* **True causal uplift:** The drug genuinely adds $+20$ points to health for every individual ($Y(1) = Y(0) + 20$). Thus:
  * For severe patients: $Y(1) = 40 + 20 = 60$.
  * For mild patients: $Y(1) = 70 + 20 = 90$.
* **True population ATE:** $\text{ATE} = E[Y(1) - Y(0)] = \mathbf{+20}$.

---

### Scenario A — Non-Exchangeable (Observational Self-Selection)

In this observational setting, sicker patients preferentially receive the drug ($X=1$): 45 of the 50 severe patients take it, while only 5 of the 50 mild patients do.

Note that the preference is strong but not absolute. Every stratum still contains patients of both kinds — $P(X=1 \mid Z = \text{severe}) = 0.9$ and $P(X=1 \mid Z = \text{mild}) = 0.1$ — so a within-stratum comparison remains possible. Lesson 13 will show that this property, **positivity**, is indispensable, and that a scenario in which *all* severe patients took the drug would be unanalysable for a reason quite separate from confounding.

#### Prior Statistical Table (Scenario A)

| Patient Group | Baseline Health ($Z$) | Count | Baseline Potential Score $Y(0)$ | Treated Potential Score $Y(1)$ | Treatment Choice ($X$) | Observed Outcome ($Y$) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Severe, treated** | Severe | 45 | 40 | 60 | $X=1$ (Treated) | $Y = Y(1) = \mathbf{60}$ |
| **Severe, control** | Severe | 5 | 40 | 60 | $X=0$ (Control) | $Y = Y(0) = \mathbf{40}$ |
| **Mild, treated** | Mild | 5 | 70 | 90 | $X=1$ (Treated) | $Y = Y(1) = \mathbf{90}$ |
| **Mild, control** | Mild | 45 | 70 | 90 | $X=0$ (Control) | $Y = Y(0) = \mathbf{70}$ |
| **Total** | — | 100 | — | — | 50 Treated / 50 Control | — |

#### Deriving Expectations from Table A

1. **Treated Group Baseline Counterfactual:** the treated group is 45 severe patients and 5 mild ones, so its average baseline is pulled far down.
   $$E[Y(0) \mid X=1] = \frac{(45 \times 40) + (5 \times 70)}{50} = \frac{1800 + 350}{50} = \frac{2150}{50} = \mathbf{43}$$
2. **Control Group Baseline:** the control group is the mirror image — 5 severe and 45 mild.
   $$E[Y(0) \mid X=0] = \frac{(5 \times 40) + (45 \times 70)}{50} = \frac{200 + 3150}{50} = \frac{3350}{50} = \mathbf{67}$$
3. **Observed Treated Average:**
   $$E[Y \mid X=1] = E[Y(1) \mid X=1] = \frac{(45 \times 60) + (5 \times 90)}{50} = \frac{2700 + 450}{50} = \frac{3150}{50} = \mathbf{63}$$
4. **Observed Control Average:**
   $$E[Y \mid X=0] = E[Y(0) \mid X=0] = \mathbf{67}$$
5. **Confounding Bias Term ($B$):**
   $$B = E[Y(0) \mid X=1] - E[Y(0) \mid X=0] = 43 - 67 = \mathbf{-24}$$
6. **Observed Association:**
   $$E[Y \mid X=1] - E[Y \mid X=0] = 63 - 67 = \mathbf{-4}$$

$$\text{Association } (-4) = \text{True ATE } (+20) + \text{Bias } B (-24)$$

Here, exchangeability **fails** because $E[Y(0) \mid X=1] \neq E[Y(0) \mid X=0]$: $43 \neq 67$. The negative confounding bias ($B = -24$) more than cancels the positive drug effect, producing a **sign reversal** in which a drug that helps every single patient by $+20$ points appears in the raw data to harm them.

---

### Scenario B — Exchangeable (Randomized Controlled Trial)

Now suppose treatment is assigned randomly by a fair coin flip ($P(X=1) = 0.5$) independently of baseline health $Z$. Exactly half of each severity group is assigned to treatment and half to control.

#### Prior Statistical Table (Scenario B)

| Patient Subgroup | Baseline Health ($Z$) | Count | Treatment Assignment ($X$) | Baseline Score $Y(0)$ | Treated Score $Y(1)$ | Observed Score ($Y$) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Severe Treated** | Severe | 25 | $X=1$ | 40 | 60 | $Y = Y(1) = \mathbf{60}$ |
| **Severe Control** | Severe | 25 | $X=0$ | 40 | 60 | $Y = Y(0) = \mathbf{40}$ |
| **Mild Treated** | Mild | 25 | $X=1$ | 70 | 90 | $Y = Y(1) = \mathbf{90}$ |
| **Mild Control** | Mild | 25 | $X=0$ | 70 | 90 | $Y = Y(0) = \mathbf{70}$ |
| **Total** | — | 100 | 50 Treated / 50 Control | — | — | — |

#### Deriving Expectations from Table B

1. **Treated Group Baseline Counterfactual:**
   $$E[Y(0) \mid X=1] = \frac{(25 \times 40) + (25 \times 70)}{50} = \frac{1000 + 1750}{50} = \frac{2750}{50} = \mathbf{55}$$
2. **Control Group Baseline:**
   $$E[Y(0) \mid X=0] = \frac{(25 \times 40) + (25 \times 70)}{50} = \frac{1000 + 1750}{50} = \frac{2750}{50} = \mathbf{55}$$
3. **Observed Treated Average:**
   $$E[Y \mid X=1] = \frac{(25 \times 60) + (25 \times 90)}{50} = \frac{1500 + 2250}{50} = \frac{3750}{50} = \mathbf{75}$$
4. **Observed Control Average:**
   $$E[Y \mid X=0] = \frac{(25 \times 40) + (25 \times 70)}{50} = \frac{1000 + 1750}{50} = \frac{2750}{50} = \mathbf{55}$$
5. **Confounding Bias Term ($B$):**
   $$B = E[Y(0) \mid X=1] - E[Y(0) \mid X=0] = 55 - 55 = \mathbf{0}$$
6. **Observed Association:**
   $$E[Y \mid X=1] - E[Y \mid X=0] = 75 - 55 = \mathbf{+20}$$

$$\text{Association } (+20) = \text{True ATE } (+20) + 0$$

Because randomization ensures $E[Y(0) \mid X=1] = E[Y(0) \mid X=0] = 55$, **exchangeability holds**, the confounding bias $B$ is identically zero, and the observed difference directly recovers the true causal ATE.

> [!TIP]
> **The Diagnostic Question:** To check whether exchangeability plausibly holds, always ask: *"Would the treated group have looked exactly like the control group if nobody had been treated?"* If the answer is no, $B \neq 0$.

---

## Visualizing Exchangeability: The Swap Test

Exchangeability gets its name from a thought experiment: **swap the labels** on the two groups and see whether anything changes.

```text
  EXCHANGEABLE (Randomized)          NON-EXCHANGEABLE (Self-Selected)
  =========================          ================================

  Group A  |  Group B                Treated grp   |  Control grp
  baseline |  baseline               (mostly severe|  (mostly mild,
    = 55   |    = 55                  base = 43)   |   base = 67)

  Swap the labels:                   Swap the labels:
  Group B treated -> 75              Controls treated  -> 87  (!= 63)
  Group A control -> 55              Treated as control-> 43  (!= 67)

  IDENTICAL RESULTS  =>  B = 0       DIFFERENT RESULTS  =>  B != 0
```

The groups are "exchangeable" precisely when this swap is invisible in the averages.

```mermaid
graph TD
    subgraph NonEx["Non-Exchangeable (Confounded by Z)"]
        X1["Treatment Choice (X)"] <-- "Correlated via Z" --> Y0["Baseline Outcome Y(0)"]
        Z1["Confounder (Z)"] --> X1
        Z1 --> Y0
    end
    subgraph Exch["Exchangeable (Randomized / Independent)"]
        Coin["Coin Flip / Randomizer"] --> X2["Treatment Assignment (X)"]
        X2 -. "NO PATH (Independent)" .- Y02["Baseline Outcome Y(0)"]
    end
```

*Caption: Left: When $X$ and $Y(0)$ are connected via common causes $Z$, exchangeability fails ($B \neq 0$). Right: When $X$ is generated by a physical randomizer, $X$ is independent of $(Y(1), Y(0))$, guaranteeing exchangeability ($B = 0$).*

---

## How We Achieve Exchangeability: RCTs vs. Observational Studies

There are two primary ways to achieve exchangeability:

### 1. Randomized Controlled Trials (RCTs) — Unconditional Exchangeability

In a randomized experiment, treatment $X$ is assigned by a coin flip, random number generator, or lottery.

* Because treatment depends strictly on a randomizer, $X$ cannot be influenced by any subject traits, health status, or baseline outcomes.
* Therefore, RCTs physically guarantee **unconditional exchangeability**: $(Y(1), Y(0)) \perp\!\!\!\perp X$.

### 2. Observational Adjustment — Conditional Exchangeability

In observational data, subjects choose their own treatment ($X$), so unconditional exchangeability rarely holds. However, if we measure all common confounders $Z$ (e.g., age, income, baseline severity), treatment assignment may become independent of potential outcomes **within strata of $Z$**:

$$(Y(1), Y(0)) \perp\!\!\!\perp X \mid Z$$

This is called **Conditional Exchangeability** (or **Conditional Ignorability**), which forms the foundation of adjustment formulas, matching, and propensity score methods (formalized in Lesson 12 onward).

### Comparing the Two Routes

| Feature | Randomization (RCT) | Observational Adjustment |
| :--- | :--- | :--- |
| **Assumption achieved** | $(Y(1), Y(0)) \perp\!\!\!\perp X$ | $(Y(1), Y(0)) \perp\!\!\!\perp X \mid Z$ |
| **How bias is removed** | Physically severed by the randomizer | Statistically blocked by conditioning on $Z$ |
| **DAG counterpart** | No arrow enters $X$ at all | All back-door paths blocked by $Z$ |
| **Vulnerability** | Non-compliance, attrition, ethics/cost | Unmeasured confounders, mis-specified $Z$ |
| **Estimator** | Simple difference in group means | Adjustment formula / Matching / Inverse Probability Weighting (IPW) |

> [!WARNING]
> **Do Not Condition on Colliders or Mediators:**
> Recall from Part 1 that conditioning is not automatically helpful. Conditioning on a **mediator** ($X \to M \to Y$) blocks part of the true causal effect (over-control bias), and conditioning on a **collider** ($X \to C \leftarrow Y$) *opens* a spurious path. Only variables that block back-door paths belong in $Z$.

---

<details>
<summary>Click to expand: Terminology Clarification — Exchangeability vs. Ignorability vs. Unconfoundedness</summary>

Different scientific disciplines use different names for the exact same mathematical independence concept $(Y(1), Y(0)) \perp\!\!\!\perp X$:

| Term | Preferred Discipline | Conceptual Intuition |
| :--- | :--- | :--- |
| **Exchangeability** | Epidemiology & Biostatistics | Treatment and control groups can be exchanged without altering expected outcomes. |
| **Ignorability** | Rubin Causal Model / Statistics | The mechanism that assigned treatment $X$ can be "ignored" when computing causal effects. |
| **Unconfoundedness** | Econometrics & Machine Learning | There are no unobserved confounders creating baseline differences between groups. |
| **No Back-Door Paths** | Pearl's Structural Causal Models | In DAG terms, all back-door paths between $X$ and $Y$ are blocked. |

All four terms describe the exact same mathematical reality: treatment assignment $X$ carries no information about potential outcomes $(Y(1), Y(0))$.

</details>

<details>
<summary>Click to expand: A Common Confusion — Independence of Potential Outcomes, Not Observed Outcomes</summary>

Exchangeability is **not** the claim that $Y \perp\!\!\!\perp X$ (that the observed outcome is independent of treatment). That would say the treatment has no effect at all!

The independence is asserted between $X$ and the **potential outcomes** $(Y(1), Y(0))$, which are fixed pre-treatment attributes of each unit:

* $Y(1)$ and $Y(0)$ describe how a unit *would* respond under each scenario. They exist before any treatment is assigned.
* $Y$ is what we actually observe *after* assignment, and it depends on $X$ by construction ($Y = Y(X)$).

So a randomized trial can simultaneously have $(Y(1), Y(0)) \perp\!\!\!\perp X$ (exchangeability holds) and a strong observed dependence between $Y$ and $X$ (the drug works). There is no contradiction.

</details>

<details>

<summary>Click to expand: Inverse Probability Weighting (IPW) (often called Inverse Probability of Treatment Weighting (IPTW)).</summary>

It is one of the most widely used statistical estimators in the Potential Outcomes framework for estimating causal effects from observational data.

### The Intuition: Creating a Synthetic Randomized Trial

In an observational study, people self-select into treatment. For example, sicker patients are more likely to take a drug ($X=1$) than healthy patients. This means the treated group has "too many" sick patients, and the control group has "too many" healthy patients.

**IPW fixes this imbalance by re-weighting individuals:**

1. **Calculate the Propensity Score:** For each person, estimate the probability that they received the treatment given their confounders $Z$:
   $$e(Z) = P(X=1 \mid Z)$$
2. **Assign Weights:**
   * A treated person who had a *low* chance of getting treated (e.g., a healthy person who took the drug anyway) gets a **large weight** ($\frac{1}{e(Z)}$).
   * A treated person who was *very likely* to get treated (e.g., a very sick person who took the drug) gets a **small weight**.
   * Control individuals are similarly weighted by $\frac{1}{1 - e(Z)}$.

By up-weighting the "underrepresented" cases and down-weighting the "overrepresented" ones, IPW mathematically creates a **pseudo-population** where treatment $X$ is no longer correlated with the confounders $Z$ — mimicking what an RCT would have produced!

---

### Why it was mentioned in the table

It was listed alongside the **Adjustment Formula** and **Matching** as the standard toolset researchers use to compute causal effects once **conditional exchangeability** and **positivity** are assumed.

</details>

---

> [!CAUTION]
> **Exchangeability is an Untestable Causal Assumption:**
> You can never test $(Y(1), Y(0)) \perp\!\!\!\perp X$ directly using observational data alone, because the required counterfactuals ($Y(0)$ for treated units, $Y(1)$ for control units) are never observed — this is the Fundamental Problem of Causal Inference from Lesson 09. Claiming exchangeability requires scientific domain knowledge, causal graph inspection, or explicit physical randomization.

---

## Self-Check

Try to answer before expanding.

<details>
<summary>Click to expand: Question 1 — Balance tables and exchangeability</summary>

**Q:** A researcher shows that the treated and control groups have identical average age, income, and education. Does this prove exchangeability holds?

**A:** No. Balance on *measured* covariates is reassuring but not sufficient. Exchangeability requires $(Y(1), Y(0)) \perp\!\!\!\perp X$, which concerns unobserved potential outcomes across *all* factors, including unmeasured confounders (e.g., genetic risk, diet, motivation, subtle physician heuristics):

* **Why it can falsify:** If a balance table reveals large baseline differences between groups, it definitively proves that the groups are not comparable ($E[Y(0) \mid X=1] \neq E[Y(0) \mid X=0]$), falsifying exchangeability on the spot.
* **Why it cannot confirm:** A clean balance table only inspects the variables the researcher chose to measure and display. It is completely blind to unmeasured confounders.

### Intuition: The Water Purity Analogy

Checking a balance table is like testing tap water exclusively for lead. If the test detects lead, you have definitively **falsified** the claim that the water is pure. But if the test finds no lead, you have **not confirmed** the water is 100% pure—it could still harbor bacteria, microplastics, or arsenic that your specific test was blind to.

Only physical **randomization** balances both measured *and* unmeasured confounders simultaneously by construction.

</details>

<details>
<summary>Click to expand: Question 2 — Sign reversal</summary>

**Q:** In Scenario A above, the drug truly helps (ATE $= +20$) yet the data show it hurts (association $= -4$). Which term is responsible, and what is its sign?

**A:** The confounding bias $B = E[Y(0) \mid X=1] - E[Y(0) \mid X=0] = 43 - 67 = -24$. Because sicker patients selected into treatment, the treated group's baseline was far worse, and this strongly negative $B$ outweighed the positive true effect. This is the classic **confounding by indication** seen throughout medical research.

</details>

---

## Summary and Key Takeaways

1. **The Core Link:** Exchangeability is the exact mathematical condition under which Confounding Bias $B = E[Y(0) \mid X=1] - E[Y(0) \mid X=0]$ equals zero.
2. **Formal Definition:** $(Y(1), Y(0)) \perp\!\!\!\perp X$ means treatment assignment is independent of *potential* outcomes — not of the observed outcome $Y$.
3. **RCT Guarantee:** Physical randomization guarantees unconditional exchangeability by construction.
4. **Observational Target:** Observational studies rely on measuring confounders $Z$ to achieve conditional exchangeability ($(Y(1), Y(0)) \perp\!\!\!\perp X \mid Z$).
5. **Untestable:** Exchangeability is an assumption justified by design or domain knowledge, never verified from data.
6. **Next Step:** Lesson 12 turns conditional exchangeability into a working estimator via the **adjustment formula**, computing the ATE stratum-by-stratum across $Z$.

---

## Further Reading

*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 4.
