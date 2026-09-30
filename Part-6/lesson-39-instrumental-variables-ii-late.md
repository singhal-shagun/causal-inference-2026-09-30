---
type: Lesson
title: "Lesson 39 — Instrumental Variables II: Principal Strata and the LATE"
description: "Instrument-based potential outcomes, compliers, always-takers, never-takers and defiers, monotonicity, and nonparametric identification of the local ATE."
resource: "/unknown/Introduction_to_Causal_Inference-Dec17_2020-Neal.pdf"
tags: [causal-inference, neal, lesson]
status: draft
generated: { by: teacher/1.0, at: 2026-09-17 }
---

# Lesson 39: Instrumental Variables II — Principal Strata and the LATE

## Where We Left Off

Lesson 38 identified the causal effect with an instrument, but only by assuming a linear outcome. Linearity quietly requires **homogeneity**: the same treatment effect for every unit. That is a strong assumption, and other versions of it are equally strong. This lesson drops it altogether. The Wald ratio of Lesson 38 still identifies something, but no longer the average effect in the whole population. It identifies the effect among the people whose treatment the instrument actually changes. Seeing why requires new potential outcomes.

*Notation.* As in Lesson 38: binary instrument $Z$, binary treatment $T$, outcome $Y$.

## Potential Outcomes for the Instrument

Think of the instrument as **encouragement**: $Z = 1$ encourages treatment, $Z = 0$ does not. Two new kinds of potential outcome describe how units respond to it.

- $T(Z{=}z)$, written $T(z)$ for short: the treatment a unit *would take* if its encouragement were $z$. Every unit has both $T(1)$ and $T(0)$.
- $Y(Z{=}z)$: the outcome a unit would have if its *encouragement* were set to $z$. This intervenes on the instrument, not the treatment.

These are different from the familiar $Y(T{=}t)$, the outcome if the *treatment* were set to $t$. To keep them apart, this lesson always writes $Y(Z{=}z)$ or $Y(T{=}t)$ in full.

Consistency applies as before: a unit with $Z = z$ has observed treatment $T = T(z)$ and observed outcome $Y = Y(Z{=}z)$.

## Principal Strata

Each unit has a pair $(T(1), T(0))$, and with binary treatment there are four possibilities. They split the population into four **principal strata**:

| Stratum | $T(1)$ | $T(0)$ | Description |
| --- | :---: | :---: | --- |
| **Compliers** | 1 | 0 | take the treatment if and only if encouraged |
| **Always-takers** | 1 | 1 | take it whether encouraged or not |
| **Never-takers** | 0 | 0 | never take it |
| **Defiers** | 0 | 1 | do the opposite of the encouragement |

**The strata have different causal graphs.** For compliers and defiers, the treatment depends on the encouragement, so the graph contains the arrow $Z \to T$. For always-takers and never-takers, the treatment does not depend on the encouragement: the arrow $Z \to T$ is absent, so encouragement has no effect on their treatment. By the exclusion restriction, encouragement can affect the outcome only through the treatment, so **encouragement has no effect on the outcome of always-takers and never-takers either**. This fact drives the proof below.

**Individual stratum membership cannot be identified.** Each observed combination of $(Z, T)$ is consistent with two strata:

| Observed | Could be |
| --- | --- |
| $Z = 0,\ T = 0$ | complier or never-taker |
| $Z = 0,\ T = 1$ | defier or always-taker |
| $Z = 1,\ T = 0$ | defier or never-taker |
| $Z = 1,\ T = 1$ | complier or always-taker |

Each unit shows only one of $T(1)$ and $T(0)$, so its stratum, which depends on both, is never observed. It is a cross-world quantity, like the joint potential outcomes of Part 4. The strata can be reasoned about, and as shown below their *sizes* can be estimated, but no individual can be labelled.

In the structural language of Lesson 17, the four strata are the four possible shapes of the mechanism $T := f_T(Z, U_T)$ for a given unit: $T$ equal to $Z$, always 1, always 0, or equal to $1 - Z$.

## The Local ATE and Monotonicity

> **Local average treatment effect (LATE),** also called the **complier average causal effect (CACE)**:
> $$\mathbb{E}\big[\,Y(Z{=}1) - Y(Z{=}0) \;\big|\; T(1) = 1,\ T(0) = 0\,\big].$$

It is the effect of encouragement among the compliers. For a complier, $T(z) = z$, so encouragement and treatment coincide. By the exclusion restriction, encouragement affects the outcome only through the treatment, so $Y(Z{=}1) = Y(T{=}T(1)) = Y(T{=}1)$ and $Y(Z{=}0) = Y(T{=}0)$. The LATE is therefore also the **treatment effect among the compliers**.

Identifying it needs one new assumption in place of linearity:

> **Monotonicity.** For every unit, $T(1) \geq T(0)$: encouragement never makes anyone *less* likely to take the treatment.

Compliers have $T(1) > T(0)$; always-takers and never-takers have $T(1) = T(0)$; defiers have $T(1) < T(0)$. So monotonicity says exactly that **there are no defiers**. It is plausible in many designs, since an invitation to a course rarely makes someone less willing to attend. It can fail in others: a clumsy health warning can provoke some people into doing the opposite.

## The Identification Theorem

> **LATE identification.** If $Z$ is an instrument, $Z$ and $T$ are binary, and monotonicity holds, then
> $$\mathbb{E}\big[Y(T{=}1) - Y(T{=}0) \mid \text{compliers}\big] = \frac{\mathbb{E}[Y \mid Z{=}1] - \mathbb{E}[Y \mid Z{=}0]}{\mathbb{E}[T \mid Z{=}1] - \mathbb{E}[T \mid Z{=}0]}.$$

This is the Wald ratio of Lesson 38, with no linearity assumed. What changes is its meaning: it is now the effect among compliers.

### Proof

**Step 1: split the effect of encouragement by stratum.** By the law of total probability over the four strata,

$$
\begin{aligned}
\mathbb{E}\big[Y(Z{=}1) - Y(Z{=}0)\big] =\ & \mathbb{E}[\,\cdot \mid \text{compliers}]\,P(\text{compliers}) + \mathbb{E}[\,\cdot \mid \text{defiers}]\,P(\text{defiers}) \\
& + \mathbb{E}[\,\cdot \mid \text{always-takers}]\,P(\text{always-takers}) + \mathbb{E}[\,\cdot \mid \text{never-takers}]\,P(\text{never-takers}),
\end{aligned}
$$

where each $\,\cdot\,$ stands for $Y(Z{=}1) - Y(Z{=}0)$.

**Step 2: always-takers and never-takers drop out.** Encouragement does not change their treatment, so by the exclusion restriction it does not change their outcome: $Y(Z{=}1) - Y(Z{=}0) = 0$ for them.

**Step 3: defiers drop out.** By monotonicity, $P(\text{defiers}) = 0$.

**Step 4: solve for the complier effect.** What remains is

$$\mathbb{E}\big[Y(Z{=}1) - Y(Z{=}0)\big] = \mathbb{E}\big[Y(Z{=}1) - Y(Z{=}0) \mid \text{compliers}\big]\, P(\text{compliers}),$$

so

$$\text{LATE} = \frac{\mathbb{E}\big[Y(Z{=}1) - Y(Z{=}0)\big]}{P(\text{compliers})}.$$

**Step 5: the numerator is observable.** The instrument has no backdoor path to the outcome (instrumental unconfoundedness), so its effect equals its association, as in a randomized experiment (Lesson 21):

$$\mathbb{E}\big[Y(Z{=}1) - Y(Z{=}0)\big] = \mathbb{E}[Y \mid Z{=}1] - \mathbb{E}[Y \mid Z{=}0].$$

This is the **intention-to-treat** effect.

**Step 6: the denominator is observable.** Among those encouraged, the units who take the treatment are those with $T(1) = 1$: compliers and always-takers. Among those not encouraged, the units who take it are those with $T(0) = 1$: always-takers and defiers, and with no defiers, only always-takers. Since the instrument is unconfounded, each group is representative of the population, so

$$
\begin{aligned}
\mathbb{E}[T \mid Z{=}1] &= P(\text{compliers}) + P(\text{always-takers}) \\
\mathbb{E}[T \mid Z{=}0] &= P(\text{always-takers}) \\
\mathbb{E}[T \mid Z{=}1] - \mathbb{E}[T \mid Z{=}0] &= P(\text{compliers})
\end{aligned}
$$

The first stage of Lesson 38 is exactly the share of compliers.

Substituting Steps 5 and 6 into Step 4 gives the theorem. □

### The Worked Example Revisited

Lesson 38's letter raised attendance from 20% to 60% and employment from 42% to 50%. Now the strata can be read off:

- **Always-takers:** $\mathbb{E}[T \mid Z{=}0] = 0.20$, so 20% attend whether or not they get the letter.
- **Compliers:** $0.60 - 0.20 = 0.40$, so 40% attend only if they get the letter.
- **Never-takers:** $1 - \mathbb{E}[T \mid Z{=}1] = 1 - 0.60 = 0.40$, so 40% never attend.

The Wald ratio, $0.08 / 0.40 = 0.20$, is now read as: **attending raises employment by 20 points among the compliers**, the 40% whose attendance the letter changes. It says nothing about the always-takers or the never-takers. The attendance effect for someone who would attend anyway, or who would never attend, may be larger or smaller.

## Costs of the LATE

Neal is candid about the costs.

1. **Monotonicity may fail.** If there are defiers, Step 3 fails, and the Wald ratio mixes the complier effect with the defiers' effect.
2. **The compliers may not be the population of interest.** A policy aimed at everyone needs the average effect for everyone. The instrument provides the average only for a group whose members cannot even be identified.
3. **The LATE depends on the instrument.** A different instrument moves a different group of people, so it has different compliers and a different LATE. Two valid instruments applied to the same population can report different effects, and both can be correct.

So the Wald formula has two readings. Under **linearity**, which implies the same effect for everyone (Lesson 38), it is the average effect in the whole population. Under **monotonicity** (this lesson), it is the effect among compliers. The formula is the same; which reading applies depends on which assumption the study can defend.

This is also the answer to a limitation noted in Lesson 21. When participants in a randomized trial do not comply with their assignment, the assignment can serve as an instrument, and the Wald ratio recovers the effect of *taking* the treatment among those who comply.

## More General Settings

Neal notes two extensions beyond binary instruments.

- **Additive unobserved confounding.** If the outcome is $Y := f(T, W) + U$, where $W$ are observed covariates, with the unobserved confounder $U$ entering additively but $f$ otherwise arbitrary, instruments can still identify $f$. Hartford et al. (Deep IV) and Xu et al. model $f$ with deep networks.
- **No additivity.** Without additive confounding, point identification fails, but instruments still give **bounds** on the effect (Pearl, *Causality*, Chapter 8). Kilbertus et al. study such settings.

As in Lesson 36: weaker assumptions give wider answers, but not no answer.

## Summary and Key Takeaways

1. Instrument-based potential outcomes, $T(z)$ and $Y(Z{=}z)$, sort units into four **principal strata**: compliers, always-takers, never-takers and defiers.
2. Encouragement has no effect on the treatment, and hence (by exclusion) none on the outcome, for always-takers and never-takers. **Monotonicity** means there are no defiers.
3. Individual stratum membership cannot be identified; stratum *sizes* can. The first stage equals the complier share.
4. Under monotonicity, the **Wald ratio identifies the LATE**, the treatment effect among compliers, with no linearity assumption.
5. The LATE depends on the instrument and may not be the effect of interest; under linearity the same ratio is the population average effect.
6. With additive confounding, instruments identify flexible outcome functions; without it, they give bounds.

**Next step:** Lesson 40 uses **time** instead: difference-in-differences, a standard tool for evaluating policies.

### Check Your Understanding

1. An always-taker and a complier with $Z = 1$ look identical in the data. Why does this not break the proof? Which step needed only the strata's sizes?
2. In the worked example, what share of people with $Z = 1$ and $T = 1$ are compliers? (Use the stratum shares.)
3. Give an example of an encouragement that could create defiers, and say what the Wald ratio would then estimate.
4. Two valid instruments applied to the same population give LATEs of 0.4 and 0.9. Is either one wrong? What does the difference suggest?

---

## Further Reading

*Ref*: Brady Neal, *Introduction to Causal Inference* (Dec 2020 draft), Chapter 9 (Nonparametric Identification of Local ATE; More General Settings); Hernán & Robins, *Causal Inference: What If*, on homogeneity assumptions; Hartford et al. (2017), *Deep IV*; Pearl, *Causality* (2009), Chapter 8.
