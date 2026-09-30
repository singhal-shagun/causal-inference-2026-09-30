---
type: Lesson
title: "Lesson 40 — Difference-in-Differences"
description: "Exploiting time: the ATT estimand, parallel trends, no pretreatment effect, the DiD identification proof, the method's major problems, and synthetic control."
resource: "/unknown/Introduction_to_Causal_Inference-Dec17_2020-Neal.pdf"
tags: [causal-inference, neal, lesson]
status: draft
generated: { by: teacher/1.0, at: 2026-09-17 }
---

# Lesson 40: Difference-in-Differences

## Where We Left Off

Every identification strategy so far compared *units*: treated with untreated, within strata or through an instrument. **Difference-in-differences** (DiD) also compares across **time**. It looks at how much each group *changed*, and in doing so it can remove confounding that stays constant over time, even if that confounding is unmeasured. It is one of the most widely used tools for evaluating policies. Neal notes that his DiD chapter (Chapter 10) is rougher than the rest of his draft; this lesson follows its argument.

*Notation.* Following Neal: $T = 1$ marks the treatment group and $T = 0$ the control group. Time is $\tau \in \{0, 1\}$: period 0 comes before the treatment is given and period 1 after. $Y_\tau(t)$ is the potential outcome at time $\tau$ under treatment status $t$.

## A Picture First

The classic design: a policy affects one group but not another. For example, one state raises its minimum wage and a neighbouring state does not (the setting of Card and Krueger's well-known 1994 study). Average employment is measured in both states, before and after. That gives four numbers.

Two simple comparisons each go wrong:

- **Before and after, in the treated state only.** This mixes the policy's effect with whatever else changed over time, such as a general economic recovery.
- **Treated against control, after the policy only.** This mixes the policy's effect with any lasting difference between the states.

The difference of the two groups' *changes* removes both. That is the idea behind DiD.

**A numerical example (hypothetical).** Average employment per store:

| | Before ($\tau = 0$) | After ($\tau = 1$) | Change |
| --- | :---: | :---: | :---: |
| Treated state ($T = 1$) | 20 | 26 | $+6$ |
| Control state ($T = 0$) | 18 | 21 | $+3$ |

- The before-and-after comparison in the treated state says $+6$. But the control state rose by 3 without any policy, so part of that $+6$ is the general trend.
- The after-only comparison says $26 - 21 = +5$. But the states already differed by 2 before the policy.
- DiD says $(26 - 20) - (21 - 18) = 6 - 3 = +3$.

The rest of the lesson states exactly when $+3$ is the causal effect.

## The Estimand: The Effect on the Treated

DiD targets a narrower quantity than the average treatment effect: the average effect **on the treated group, after treatment**. This is the ATT of Lessons 22 and 30:

$$\mathbb{E}\big[\,Y_1(1) - Y_1(0) \;\big|\; T = 1\,\big].$$

Neal first points out that the ATT needs a weaker assumption than the ATE even without time. Full unconfoundedness, $(Y(1), Y(0)) \perp\!\!\!\perp T$, identifies the ATE. For the ATT it is enough that $Y(0) \perp\!\!\!\perp T$:

$$
\begin{aligned}
\mathbb{E}[Y(1) - Y(0) \mid T = 1] &= \mathbb{E}[Y(1) \mid T = 1] - \mathbb{E}[Y(0) \mid T = 1] \\
&= \mathbb{E}[Y \mid T = 1] - \mathbb{E}[Y(0) \mid T = 1] && \text{consistency} \\
&= \mathbb{E}[Y \mid T = 1] - \mathbb{E}[Y(0) \mid T = 0] && Y(0) \perp\!\!\!\perp T \\
&= \mathbb{E}[Y \mid T = 1] - \mathbb{E}[Y \mid T = 0] && \text{consistency}
\end{aligned}
$$

Only the untreated potential outcome has to be unconfounded, because the treated group's own outcome under treatment is observed directly. DiD uses a different identifying assumption again, one about *changes* over time.

## The Assumptions

**Consistency, with time.** At each time, the observed outcome is the potential outcome for the unit's treatment status: $Y_\tau = Y_\tau(T)$. So:

- $\mathbb{E}[Y_1(1) \mid T = 1] = \mathbb{E}[Y_1 \mid T = 1]$: the treated group's observed outcome after treatment;
- $\mathbb{E}[Y_\tau(0) \mid T = 0] = \mathbb{E}[Y_\tau \mid T = 0]$ at both times: the control group's observed outcomes.

Two quantities are *not* given by consistency. One is $\mathbb{E}[Y_1(0) \mid T = 1]$, what the treated group would have had after the policy without it; this is what DiD must reconstruct. The other is subtler: the treated group's observed *pre*-period outcome is $\mathbb{E}[Y_0(1) \mid T = 1]$, because its treatment status is 1 even though the treatment has not yet been given. As in the rest of the course, no interference is also assumed, now at each time.

> **Parallel trends.**
> $$\mathbb{E}\big[Y_1(0) - Y_0(0) \mid T = 1\big] = \mathbb{E}\big[Y_1(0) - Y_0(0) \mid T = 0\big].$$

Without treatment, the treated group's outcome would have changed over time by the same amount, on average, as the control group's did. Neal describes it as unconfoundedness for a *difference*, $(Y_1(0) - Y_0(0)) \perp\!\!\!\perp T$, in place of unconfoundedness for a level, $Y(0) \perp\!\!\!\perp T$. The groups may differ in their *levels*; they must not differ in their untreated *trends*.

> **No pretreatment effect.**
> $$\mathbb{E}\big[Y_0(1) - Y_0(0) \mid T = 1\big] = 0.$$

The treatment has no effect on the treated group before it is given. That sounds obviously true, but it fails if people **anticipate** the treatment and change their behaviour in advance.

> [!WARNING]
> **Parallel trends is an assumption about counterfactual trends.** Showing that the two groups' trends were parallel in several periods *before* treatment is useful evidence, and it is standard practice. But it cannot prove the assumption, which concerns what would have happened *after* the treatment date without treatment. A divergence can begin exactly at the treatment date. A well-known example is **Ashenfelter's dip** (Ashenfelter, 1978): people tend to join job-training programs just after a drop in their earnings, so their earnings would have recovered anyway, and the treated group's untreated trend differs from the control group's. Report pre-period trends, but also argue for parallel trends from knowledge of the setting.

## The Identification Result

> **Difference-in-differences identification.** Given consistency, parallel trends and no pretreatment effect,
> $$\mathbb{E}\big[Y_1(1) - Y_1(0) \mid T{=}1\big] = \Big(\mathbb{E}[Y_1 \mid T{=}1] - \mathbb{E}[Y_0 \mid T{=}1]\Big) - \Big(\mathbb{E}[Y_1 \mid T{=}0] - \mathbb{E}[Y_0 \mid T{=}0]\Big).$$

The treated group's change minus the control group's change. Estimating it needs only the four group means.

### Proof

**Step 1: split the effect.**

$$\mathbb{E}[Y_1(1) - Y_1(0) \mid T{=}1] = \mathbb{E}[Y_1(1) \mid T{=}1] - \mathbb{E}[Y_1(0) \mid T{=}1] = \mathbb{E}[Y_1 \mid T{=}1] - \mathbb{E}[Y_1(0) \mid T{=}1],$$

using consistency for the first term. The second term is the counterfactual to reconstruct.

**Step 2: reconstruct it from parallel trends.** Rearranging the parallel-trends assumption:

$$
\begin{aligned}
\mathbb{E}[Y_1(0) \mid T{=}1] &= \mathbb{E}[Y_0(0) \mid T{=}1] + \mathbb{E}[Y_1(0) \mid T{=}0] - \mathbb{E}[Y_0(0) \mid T{=}0] && \text{parallel trends} \\
&= \mathbb{E}[Y_0(0) \mid T{=}1] + \mathbb{E}[Y_1 \mid T{=}0] - \mathbb{E}[Y_0 \mid T{=}0] && \text{consistency, control group} \\
&= \mathbb{E}[Y_0(1) \mid T{=}1] + \mathbb{E}[Y_1 \mid T{=}0] - \mathbb{E}[Y_0 \mid T{=}0] && \text{no pretreatment effect} \\
&= \mathbb{E}[Y_0 \mid T{=}1] + \mathbb{E}[Y_1 \mid T{=}0] - \mathbb{E}[Y_0 \mid T{=}0] && \text{consistency, treated group}
\end{aligned}
$$

The third line is where no pretreatment effect is needed. $\mathbb{E}[Y_0(0) \mid T{=}1]$ is the treated group's pre-period outcome *without* treatment. What was observed is its pre-period outcome under its actual status, $\mathbb{E}[Y_0(1) \mid T{=}1]$. The assumption says the two are equal.

**Step 3: substitute into Step 1.**

$$
\begin{aligned}
\mathbb{E}[Y_1(1) - Y_1(0) \mid T{=}1] &= \mathbb{E}[Y_1 \mid T{=}1] - \mathbb{E}[Y_0 \mid T{=}1] - \mathbb{E}[Y_1 \mid T{=}0] + \mathbb{E}[Y_0 \mid T{=}0] \\
&= \Big(\mathbb{E}[Y_1 \mid T{=}1] - \mathbb{E}[Y_0 \mid T{=}1]\Big) - \Big(\mathbb{E}[Y_1 \mid T{=}0] - \mathbb{E}[Y_0 \mid T{=}0]\Big). \qquad \square
\end{aligned}
$$

In the numerical example, the reconstructed counterfactual is $20 + 21 - 18 = 23$: without the policy, the treated state would have reached 23. It actually reached 26, so the effect on the treated is $26 - 23 = +3$.

## Why DiD Removes Some Unmeasured Confounding

Suppose an unmeasured factor makes the treated group different from the control group, such as a state's industrial mix, but its effect on the outcome is the same in both periods. Then it shifts the treated group's level in both periods by the same amount, and that shift cancels when the group's change is computed. Likewise, anything that changes both groups' outcomes equally over time, such as a national economic shock, cancels when one group's change is subtracted from the other's.

One simple structure that makes parallel trends hold: the untreated outcome is the sum of a part that depends on group and time only through separate terms, and a part that depends on a unit-level factor $U$ that does not change over time, $Y_\tau(0) = g(\tau) + h(U)$. Then each group's untreated change is $g(1) - g(0)$, the same for both groups, whatever $U$ is and however strongly it affects group membership. Parallel trends fails when the unmeasured factor itself changes differently over time in the two groups, or when group and time interact.

The course now has four responses to unmeasured confounding: bounds (Lesson 36), sensitivity analysis (Lesson 37), instruments (Lessons 38–39), and DiD (this lesson). Each rests on a different assumption.

## Major Problems

Neal lists two.

1. **Parallel trends often fails.** A common response is to condition on covariates $X$ and assume **controlled parallel trends**:
   $$\mathbb{E}\big[Y_1(0) - Y_0(0) \mid T = 1, X\big] = \mathbb{E}\big[Y_1(0) - Y_0(0) \mid T = 0, X\big].$$
   This is weaker, but it can still fail. For example, if the untreated outcome's structural equation contains an interaction between group and time, the two groups' untreated changes differ by that interaction term, so parallel trends fails unless the term happens to be zero.
2. **Parallel trends depends on the scale of the outcome.** It is an assumption about *differences*, so it does not survive a change of scale. Trends can be parallel for the outcome itself and not for its logarithm, or the other way round. Neal therefore calls parallel trends, and DiD, **semi-parametric** rather than fully nonparametric. The choice of scale is part of the causal assumption and needs to be defended, not chosen by habit.

Anticipation, which breaks the no-pretreatment-effect assumption, is a third problem to keep in mind.

## An Extension: Synthetic Control

Neal's DiD week included a guest lecture by Alberto Abadie on **synthetic control**, a method that is not in Neal's book. It addresses a common situation where DiD is fragile: a single treated unit, such as one state that passes a law, and no single control unit whose trend is believably parallel to it.

**The idea.** Instead of choosing one control unit, or averaging all of them equally, build a **weighted combination** of control units that tracks the treated unit closely *before* the treatment. The weights are non-negative and sum to 1, so the combination, the "synthetic" unit, is a blend of real units. After treatment, the synthetic unit shows what would have happened without treatment, and the gap between the treated unit and its synthetic twin estimates the effect.

**A small numerical example (hypothetical).** One treated region and two controls, A and B, observed in two periods before the policy and one after:

| | Before, period 1 | Before, period 2 | After |
| --- | :---: | :---: | :---: |
| Treated region | 10 | 12 | 11 |
| Control A | 8 | 9 | 9 |
| Control B | 14 | 18 | 24 |

**Step 1: choose the weights.** Look for a weight $w$ on A (and $1 - w$ on B) that reproduces the treated region's pre-period values. Period 1 requires

$$8w + 14(1 - w) = 10 \quad\Longrightarrow\quad 14 - 6w = 10 \quad\Longrightarrow\quad w = \tfrac{2}{3}.$$

Check period 2: $\tfrac{2}{3} \times 9 + \tfrac{1}{3} \times 18 = 6 + 6 = 12$. The synthetic region matches both pre-period values.

**Step 2: predict the untreated outcome after the policy.** $\tfrac{2}{3} \times 9 + \tfrac{1}{3} \times 24 = 6 + 8 = 14$.

**Step 3: estimate the effect.** The treated region reached 11, against 14 for its synthetic twin, so the estimated effect is $11 - 14 = -3$.

With real data, an exact match is rarely possible; the weights are chosen to make the pre-period fit as close as possible, often using covariates as well as past outcomes.

**How it relates to DiD.** DiD compares the treated group with an *equally weighted* control group and assumes their untreated trends are parallel. Synthetic control *chooses* the weights so that the pre-treatment paths match, which makes the comparison more credible when no single control resembles the treated unit. It still rests on an untestable assumption: that the blend which tracked the treated unit before the policy would have kept tracking it afterwards. A good pre-period fit is evidence for that, not proof, for the same reason that parallel pre-trends do not prove parallel trends.

The best-known applications are Abadie and Gardeazabal's (2003) study of terrorism and the Basque Country's economy, and Abadie, Diamond and Hainmueller's (2010) study of California's 1988 tobacco-control program, where the synthetic California was a weighted blend of other US states.

## Summary and Key Takeaways

1. DiD estimates the **effect on the treated after treatment** from four group-by-time means.
2. The ATT needs weaker assumptions than the ATE: $Y(0) \perp\!\!\!\perp T$ already suffices without time. DiD replaces that with an assumption about changes.
3. **Parallel trends:** without treatment, the treated group would have changed like the control group. It concerns untreated trends, not levels, and cannot be proved from pre-period data.
4. **No pretreatment effect:** the treatment does nothing before it is given. Anticipation breaks it.
5. **Result:** effect on the treated = treated group's change − control group's change. In the example, $6 - 3 = 3$.
6. Unmeasured confounders whose effect is constant over time cancel. Confounders that change the groups' trends differently do not.
7. Parallel trends is scale-dependent, making DiD semi-parametric.
8. **Synthetic control** replaces the equally weighted control group with a weighted blend of control units chosen to match the treated unit before treatment. It suits a single treated unit, and rests on the assumption that the blend would have kept tracking it.

**Part 6 is complete. Next step:** Part 7 relaxes the largest assumption of the whole course, that the causal graph is known, and asks whether the graph can be learned from data.

### Check Your Understanding

1. In the numerical example, suppose a national shock added 5 to employment in both states after the policy. Recompute all three comparisons (before-after, after-only, DiD). Which of them change?
2. The proof used no pretreatment effect to replace $\mathbb{E}[Y_0(0) \mid T{=}1]$. Why can consistency alone not supply this quantity, even though it refers to a period before the treatment?
3. Suppose parallel trends holds in levels, so that without the policy the treated state would have gone from 20 to 23, while the control state went from 18 to 21. Compute each state's untreated change in $\log$ employment. Does parallel trends also hold in logarithms?
4. Give an example of anticipation that would make the no-pretreatment-effect assumption fail.
5. In the synthetic-control example, compute the DiD estimate that uses the equally weighted average of A and B as the control group, comparing period 2 with the post-policy period. Why does it differ from $-3$?

---

## Further Reading

*Ref*: Brady Neal, *Introduction to Causal Inference* (Dec 2020 draft), Chapter 10 (Difference in Differences); Card & Krueger (1994), *Minimum wages and employment*; Ashenfelter (1978), on earnings before program entry; Bertrand, Duflo & Mullainathan (2004), *How much should we trust differences-in-differences estimates?*; Abadie & Gardeazabal (2003), *The economic costs of conflict: a case study of the Basque Country*; Abadie, Diamond & Hainmueller (2010), *Synthetic control methods for comparative case studies: estimating the effect of California's tobacco control program*. Synthetic control was the subject of Alberto Abadie's guest lecture in Neal's course.
