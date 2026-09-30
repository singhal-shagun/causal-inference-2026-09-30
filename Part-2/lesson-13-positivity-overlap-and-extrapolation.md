---
type: Lesson
title: "Lesson 13 — Positivity: Overlap and Extrapolation"
description: "Why every stratum needs both treated and control units, and what happens to the adjustment formula when they are missing."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: human:shagun-with-ai, at: 2026-07-22 }
---

# Lesson 13: Positivity: Overlap and Extrapolation

## Where We Left Off

**Lesson 12** derived the adjustment formula and showed it recovers the true ATE:

$$\text{ATE} = \sum_z \Big( \underbrace{E[Y \mid X=1, Z=z]}_{\text{needs treated units}} - \underbrace{E[Y \mid X=0, Z=z]}_{\text{needs control units}} \Big) P(Z=z)$$

That derivation quietly assumed something we never stated: that **both** of those conditional means actually exist for **every** stratum $z$. If some stratum contains no treated patients — or no untreated patients — one half of the subtraction is missing and the sum cannot be evaluated.

This lesson formalizes that requirement — and in doing so it asks a different kind of question from the one Lessons 10–12 were answering. Those lessons asked whether the adjustment formula is **correct**: Lesson 11 showed that exchangeability drives the confounding bias $B$ to zero, and Lesson 12 weakened it to the conditional form and derived the formula that recovered the true $+20$ effect from data that appeared to show $-4$. Every step of that argument concerned whether the subtraction above yields the *right* number.

This lesson asks whether it yields a number at all. In a stratum containing no treated units, $E[Y \mid X=1, Z=z]$ is an average over zero people; the formula does not return a wrong answer, it returns no answer. Correctness and computability are separate properties, and Lesson 12 secured only the first.

---

## The Positivity Assumption

The **Positivity Assumption** (also known as the **Overlap Assumption**) states that every individual in the population has a non-zero probability of receiving *each* treatment level, given their confounders:

$$0 < P(X=1 \mid Z=z) < 1 \quad \text{for all } z \text{ where } P(Z=z) > 0$$

Note the **strict inequalities on both sides**. The next section explains why both bounds are needed.

---

## Why Positivity Matters: Both Bounds Are Fatal

The adjustment formula needs both quantities in every stratum. Each boundary case destroys one of them.

### Case 1 — $P(X=1 \mid Z=z) = 0$ (nobody in the stratum is treated)

If $P(X=1 \mid Z=z) = 0$ for a particular subgroup, then we cannot calculate the conditional mean $E[Y \mid X=1, Z=z]$ because we have no data for it.

Mathematically, this forces a division by zero:

$$E[Y \mid X=1, Z=z] = \frac{P(Y, X=1, Z=z)}{\underbrace{P(X=1, Z=z)}_{=\,0}} \quad \text{(undefined)}$$

*Example:* Doctors in the study cohort never take the drug themselves, so $P(X=1 \mid Z = \text{doctor}) = 0$. There is no treated arm to observe.

### Case 2 — $P(X=1 \mid Z=z) = 1$ (everybody in the stratum is treated)

Here $P(X=0 \mid Z=z) = 0$, so the *other* conditional mean collapses:

$$E[Y \mid X=0, Z=z] = \frac{P(Y, X=0, Z=z)}{\underbrace{P(X=0, Z=z)}_{=\,0}} \quad \text{(undefined)}$$

*Example:* A certain drug has been tested only on terminally ill patients, so $P(X=1 \mid Z = \text{terminal}) = 1$. There is no control arm to compare against.

> [!IMPORTANT]
> **The failure is symmetric.** A stratum with no treated units and a stratum with no untreated units are equally useless: in both cases one side of the within-stratum contrast is missing. This is precisely why positivity demands the **open interval** $(0, 1)$ rather than merely $P(X=1 \mid Z=z) > 0$.

### Illustration Across Strata

| Stratum $Z$ | $P(X=1 \mid Z)$ | Treated units | Control units | Stratum effect computable? |
| :--- | :---: | :---: | :---: | :--- |
| Severe | $0.5$ | 25 | 25 | Yes |
| Mild | $0.8$ | 40 | 10 | Yes (noisy, but defined) |
| Doctors | $0.0$ | 0 | 50 | **No — treated arm missing** |
| Terminally Ill | $1.0$ | 50 | 0 | **No — control arm missing** |

The Mild row deserves a second look, because it marks a boundary the formal statement does not capture. With $P(X=1 \mid Z = \text{mild}) = 0.8$, positivity holds: $0.8$ sits strictly inside $(0,1)$, both conditional means are defined, and the contrast is computable. But it rests on 10 control patients against 40 treated ones, so the control mean is estimated from a thin slice and carries a correspondingly wide standard error.

This is the distinction between **theoretical positivity** — the probability is strictly between $0$ and $1$, so the quantity exists — and **practical positivity**, which asks whether enough units sit on each side to estimate it with useful precision. Theoretical positivity is the assumption identification requires; practical positivity is what determines whether the resulting estimate is worth reporting. A propensity of $0.99$ satisfies the former and fails the latter, and as the propensity drifts toward either boundary the estimate degrades continuously long before it becomes undefined. The failure modes catalogued below are the endpoints of that drift, not a separate phenomenon.

---

## Positivity Is Not Implied by Consistency

A natural question: if a stratum has no treated units, isn't that already ruled out by **consistency**? No — the two assumptions govern entirely different things.

**Consistency** is a statement about a *single unit*:

$$Y_u = Y_u(x) \quad \text{whenever} \quad X_u = x$$

It says the outcome we recorded for a unit is the potential outcome corresponding to the treatment that unit actually received. It rules out **hidden versions of treatment** (e.g. $X=1$ silently meaning 10 mg for some patients and 50 mg for others). It says nothing about *how many* units received each level.

### The Two Can Fail Independently

Return to the doctors stratum, $P(X=1 \mid Z = \text{doctor}) = 0$:

* **Consistency holds perfectly.** Every doctor in the cohort received the control, and for each one $Y = Y(0)$. There is a single well-defined version of the control. Nothing is violated.
* **Positivity fails.** There are zero treated doctors, so $E[Y \mid X=1, Z=\text{doctor}]$ does not exist in the data.

The converse also occurs: a stratum can have a healthy 50/50 split (positivity fine) while the treatment arm secretly mixes two dosages (consistency violated).

### Division of Labour Among the Three Assumptions

| Assumption | Question it answers | Failure mode |
| :--- | :--- | :--- |
| **Consistency** | Is the observed $Y$ the *right* potential outcome? | Ambiguous treatment definition |
| **Exchangeability** | Are treated and control comparable given $Z$? | Confounding bias $B \neq 0$ |
| **Positivity** | Does the data *contain* both arms in every stratum? | Undefined conditional means |

> [!NOTE]
> **A Useful Framing:** Exchangeability and consistency make the adjustment formula **correct**. Positivity makes it **computable**. All three are required, and none implies another.

---

## Positivity Is Not Implied by Exchangeability Either

The same question arises for exchangeability, and after two lessons spent establishing it as the assumption that rescues observational data, it arrives with some force: positivity may be a new requirement, but exchangeability has already been verified — surely a stratum missing an entire arm would have surfaced there?

It would not, and the reason is sharper than the one just given for consistency. Exchangeability is not merely insufficient for positivity; it is **trivially satisfied in exactly the strata where positivity fails**. A single fact — that $X$ does not vary within such a stratum — both violates positivity and makes exchangeability free. The two are not independent failures: one manufactures the other's false reassurance, which is why the assurance earned in Lessons 11 and 12 does not carry over to this question.

### The Degenerate-Variable Trap

A **constant random variable is independent of everything**. If $B$ always equals $b_0$, then for any event $A$:

$$P(A, B = b_0) = P(A) = P(A) \cdot \underbrace{P(B = b_0)}_{=\,1}$$

Independence holds by definition.

Now apply this to a stratum where positivity fails. Suppose $P(X=1 \mid Z = \text{doctor}) = 0$. Within that stratum $X$ is constant — always $0$. Therefore:

$$(Y(1), Y(0)) \perp\!\!\!\perp X \mid Z = \text{doctor}$$

holds **vacuously**. There is no dependence to detect, because $X$ does not vary at all.

The word *vacuously* is carrying real weight here. A statement is **vacuously true** when it holds only because the situation it describes never arises, rather than because a genuine constraint was tested and satisfied — in the manner of *"every unicorn in this room is purple,"* which is true precisely because no unicorn in the room can fail to be purple, and which tells us nothing about unicorns or about the room. Exchangeability is a promise about a *comparison*: among units sharing the same $Z$, the treated and control groups are interchangeable. The doctors stratum holds 50 controls and zero treated units, so there is no comparison to perform and no way for the promise to be broken. It is kept by the absence of anything capable of breaking it.

That makes the failure diagnostic rather than merely philosophical. Exchangeability stops functioning as an assumption in exactly the strata where its verdict would matter most, and under inspection a vacuous "yes" is indistinguishable from a substantive one.

> [!IMPORTANT]
> The stratum is *perfectly* exchangeable and *completely* uninformative at the same time. Exchangeability is a statement about the **absence of bias**; positivity is a statement about the **presence of information**. You can have flawless absence of bias and zero information simultaneously.

### A Real Design Where Exactly This Happens

Consider a policy rule: **"Prescribe the drug to every patient over 65."** Let $Z = \text{age}$. Then:

$$P(X=1 \mid Z = 70) = 1, \qquad P(X=1 \mid Z = 60) = 0$$

* **Conditional exchangeability given age?** Holds trivially — treatment is a deterministic function of $Z$, so within any single age there is no residual variation to be confounded.
* **Positivity?** Violated at *every* age. Not one value of $Z$ contains both treated and untreated patients.
* **Identifiable by adjustment?** No. The adjustment formula has no stratum with a computable contrast.

This is the classic **regression discontinuity** setup, and it explains why regression discontinuity requires its own machinery — a local-randomization argument in a narrow window around the cutoff — rather than plain back-door adjustment. Standard adjustment is powerless because positivity fails globally.

### Separating the Two Assumptions

Every example so far has been a single *stratum*: the doctors, the terminally ill, the patients aged exactly 70. Step back now to the level of the *design* that produced those strata, and take the two assumptions as a pair. Each may hold or fail independently, giving four combinations — and running through all four is what establishes that neither assumption constrains the other.

Start with the design where both assumptions hold: the randomized trial of Lesson 11 Scenario B, in which treatment was assigned by a fair coin rather than by anything about the patient. That one decision settles both assumptions at once — but it settles them through two *different* properties of the coin, and separating them is the whole point.

The coin is **blind**. Its outcome does not depend on the patient's severity, on their unmeasured characteristics, or on their potential outcomes, so the group that ends up treated is comparable to the group that ends up control. That is exchangeability.

The coin is also **two-sided**. Its probability $p$ lies strictly between $0$ and $1$, so every patient could genuinely have landed in either arm, and both arms fill up in every stratum. That is positivity.

Blindness and two-sidedness are independent properties. One concerns whether assignment is *tilted* by anything about the unit; the other concerns whether assignment *varies* at all. A single act of randomization happens to supply both, which is why they feel like one idea rather than two — but any design that is not a randomized trial has to earn them separately, and most earn only one:

| Assignment mechanism | Where we met it | Exchangeability | Positivity | Identifiable? |
| :--- | :--- | :---: | :---: | :---: |
| Coin flip, $p = 0.5$ | The randomized trial of Lesson 11 | Yes | Yes | Yes |
| Coin flip, $p = 0.9$ | The Mild stratum above, pushed further | Yes | Yes | Yes (imprecise) |
| Deterministic rule on $Z$ | The over-65 rule, just above | Yes (vacuously) | **No** | **No** |
| Self-selection on unmeasured $U$ | Scenario A of Lesson 11, with the confounder left unmeasured | **No** | Yes | **No** |

Only the top two rows are usable, and the interesting content sits in the bottom two. Row 3 is the case this section was built to establish: exchangeability at its most perfect, and the study still hopeless. Row 4 is its mirror image and the reason positivity cannot be treated as the assumption that matters most — here both arms are fully populated in every stratum, so positivity is in perfect health, and the study fails anyway because the groups filling those arms are not comparable. Each assumption is fatal on its own, and neither one's success says anything about the other's.

Row 2 is the reminder that these are not four discrete cases but corners of a continuum. Nudge $p$ from $0.9$ toward $1$ and row 2 degrades smoothly into row 3: the estimate grows noisier and noisier, and only at the boundary does it stop existing altogether.

---

## Overlap vs. Extrapolation

When positivity is violated, we have a **lack of overlap** between the treatment and control groups.

```text
   COVARIATE Z:   severe    moderate    mild    terminal   doctors
                  ------    --------    ----    --------   -------
   Treated  X=1     25         30        40        50          0
   Control  X=0     25         20        10         0         50
                  ------    --------    ----    --------   -------
   Overlap?         OK         OK        OK      FAILS      FAILS
                                                 (no          (no
                                                control)    treated)
```

Only the first three strata contribute a usable within-stratum contrast. The last two sit in the **region of non-overlap**.

If researchers still try to estimate the causal effect in regions without overlap, they must perform **Extrapolation** — assuming that a model fitted where data *does* exist continues to hold where it does not. Extrapolation is highly sensitive to modeling assumptions and frequently leads to severe bias.

### Why Extrapolation Is Dangerous

A regression model will happily produce a number for $E[Y \mid X=1, Z=\text{doctor}]$ even though no treated doctor exists in the data. The number is not an estimate — it is a projection of the model's functional form (linear, quadratic, etc.) into empty space. Two analysts fitting different but equally plausible models will get different answers, with nothing in the data to adjudicate between them.

> [!CAUTION]
> **Check for Overlap:** Before adjusting for confounders, always inspect the distribution of $P(X=1 \mid Z)$ across strata. Strata pinned at $0$ or $1$ cannot contribute to the estimate, and silently including them means reporting an extrapolation as if it were a measurement.

### Practical Responses to Non-Overlap

| Strategy | What it does | Cost |
| :--- | :--- | :--- |
| **Trim / restrict** | Drop strata where $P(X=1 \mid Z)$ is $0$, $1$, or extreme | The estimand changes — you now estimate the ATE for the *overlapping* sub-population only |
| **Coarsen $Z$** | Merge fine strata into broader ones to recover overlap | Risks reintroducing confounding within the coarser strata |
| **Report the target explicitly** | State the sub-population the estimate applies to | Honest, but narrower external validity |
| **Extrapolate via model** | Assume a functional form extends into empty regions | Untestable; highly model-dependent |

> [!TIP]
> **Positivity Redefines the Estimand.** When overlap fails, the honest response is usually not "estimate anyway" but "estimate for whom you *can*." An ATE over a restricted population is a real quantity; an extrapolated ATE over the full population may be fiction.

---

## Summary and Key Takeaways

1. **The Requirement:** $0 < P(X=1 \mid Z=z) < 1$ for every stratum with $P(Z=z) > 0$.
2. **Both Bounds Matter:** $P = 0$ removes the treated arm; $P = 1$ removes the control arm. Each leaves the within-stratum contrast undefined.
3. **Theoretical vs. Practical Positivity:** Strict inequality is enough to make the contrast *exist*; it is not enough to make it *precise*. A propensity of $0.99$ is formally admissible and practically useless, and the estimate degrades continuously as the propensity approaches either boundary.
4. **Independent of Consistency:** Consistency governs whether $Y$ maps to the right potential outcome; positivity governs whether the data contains both arms. Neither implies the other.
5. **Independent of Exchangeability:** Worse — exchangeability holds *vacuously* wherever positivity fails, because a constant $X$ is independent of everything. Perfect exchangeability plus zero information is a real and common combination.
6. **Neither Assumption Ranks Above the Other:** A deterministic rule on $Z$ gives perfect exchangeability and no positivity; self-selection on an unmeasured $U$ gives perfect positivity and no exchangeability. Both are unidentifiable, so neither assumption's success licenses any inference about the other.
7. **Correct vs. Computable:** Exchangeability and consistency make the adjustment formula *correct*; positivity makes it *computable*.
8. **Non-Overlap Forces a Choice:** Either restrict the estimand to the overlapping sub-population, or extrapolate and accept model dependence.
9. **Next Step:** Lesson 14 completes the assumption set with **SUTVA** — no interference between units, and a single well-defined version of each treatment.

---

### Further Reading

*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 4.
