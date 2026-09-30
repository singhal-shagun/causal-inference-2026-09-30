---
type: Lesson
title: "Lesson 29 — Counterfactuals in Experiments and Linear Models"
description: "What experiments and observational studies can reveal about counterfactuals: the synthetic population, the randomized-trial table, and Theorem 4.3.2 in linear models."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: teacher/1.0, at: 2026-09-17 }
---

# Lesson 29: Counterfactuals in Experiments and Linear Models

## Where We Left Off

Lessons 27–28 computed counterfactuals from fully specified models. In practice, experiments and observational studies do not provide models; they provide data. This lesson asks what data can reveal about counterfactuals, first in a randomized experiment and then in linear models, where a shortcut makes counterfactuals much easier to compute.

## The Synthetic Population

Return to the encouragement design of Lesson 27 (Primer Model 4.1):

$$
\begin{aligned}
X &:= U_X \\
H &:= 0.5\, X + U_H \\
Y &:= 0.7\, X + 0.4\, H + U_Y
\end{aligned}
$$

where $X$ is time in the remedial program, $H$ is homework and $Y$ is the exam score. Suppose ten students take part, Joe among them. Each student $i$ has background factors $U_i = (U_X, U_H, U_Y)$. Because the model is fully specified, each student's factors determine everything about them: what was observed, and what would have happened under any intervention.

| Student | $U_X$ | $U_H$ | $U_Y$ | $X$ | $Y$ | $H$ | $Y_0$ | $Y_1$ | $H_0$ | $H_1$ | $Y_{00}$ |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 1 (Joe) | 0.5 | 0.75 | 0.75 | 0.5 | 1.50 | 1.0 | 1.05 | 1.95 | 0.75 | 1.25 | 0.75 |
| 2 | 0.3 | 0.1 | 0.4 | 0.3 | 0.71 | 0.25 | 0.44 | 1.34 | 0.1 | 0.6 | 0.4 |
| 3 | 0.5 | 0.9 | 0.2 | 0.5 | 1.01 | 1.15 | 0.56 | 1.46 | 0.9 | 1.4 | 0.2 |
| 4 | 0.6 | 0.5 | 0.3 | 0.6 | 1.04 | 0.8 | 0.50 | 1.40 | 0.5 | 1.0 | 0.3 |
| 5 | 0.5 | 0.8 | 0.9 | 0.5 | 1.67 | 1.05 | 1.22 | 2.12 | 0.8 | 1.3 | 0.9 |
| 6 | 0.7 | 0.9 | 0.3 | 0.7 | 1.29 | 1.25 | 0.66 | 1.56 | 0.9 | 1.4 | 0.3 |
| 7 | 0.2 | 0.3 | 0.8 | 0.2 | 1.10 | 0.4 | 0.92 | 1.82 | 0.3 | 0.8 | 0.8 |
| 8 | 0.4 | 0.6 | 0.2 | 0.4 | 0.80 | 0.8 | 0.44 | 1.34 | 0.6 | 1.1 | 0.2 |
| 9 | 0.6 | 0.4 | 0.3 | 0.6 | 1.00 | 0.7 | 0.46 | 1.36 | 0.4 | 0.9 | 0.3 |
| 10 | 0.3 | 0.8 | 0.3 | 0.3 | 0.89 | 0.95 | 0.62 | 1.52 | 0.8 | 1.3 | 0.3 |

*Primer Table 4.3. The $U$ values were drawn uniformly from $[0, 1]$; every other column follows from the model.*

**How a row is filled in, for Joe.**

- *Observed values:* $X = U_X = 0.5$; $H = 0.5 \times 0.5 + 0.75 = 1.0$; $Y = 0.7 \times 0.5 + 0.4 \times 1.0 + 0.75 = 1.50$.
- *Under $X = 0$:* $H_0 = 0 + 0.75 = 0.75$, and $Y_0 = 0 + 0.4 \times 0.75 + 0.75 = 1.05$.
- *Under $X = 1$:* $H_1 = 0.5 + 0.75 = 1.25$, and $Y_1 = 0.7 + 0.4 \times 1.25 + 0.75 = 1.95$.
- *Under $X = 0$ and $H = 0$:* $Y_{00} = 0 + 0 + 0.75 = 0.75$, which is just $U_Y$.

The columns $X$, $H$, $Y$ describe what an observational study would record. The columns $Y_0$, $Y_1$, $H_0$, $H_1$ describe what would happen under the two treatments. Further columns such as $Y_{00}$ can be added without limit. The model therefore defines a **synthetic population**: a complete table from which any counterfactual question can be answered by counting rows, as in Lesson 28.

Notice one more feature: in every row, $Y_1 - Y_0 = 0.9$. In this linear model, the program has exactly the same effect on every student.

## What an Experiment Actually Sees

Table 4.3 is never available in practice. It was deduced from a model that tells us each student's background factors. Without such a model, very little can be learned about an individual's $Y_1$ and $Y_0$ from their observed behaviour. The only link is the consistency rule of Lesson 27: $Y_1 = Y$ for a student who received $X = 1$, and $Y_0 = Y$ for one who received $X = 0$.

Much more can be learned at the population level. Run a randomized experiment: assign students 1, 5, 6, 8 and 10 to $X = 0$ and the other five to $X = 1$. Each student then reveals exactly one potential outcome:

| Student | $Y_0$ (true) | $Y_1$ (true) | $Y_0$ (observed) | $Y_1$ (observed) |
| :---: | :---: | :---: | :---: | :---: |
| 1 | 1.05 | 1.95 | 1.05 | ■ |
| 2 | 0.44 | 1.34 | ■ | 1.34 |
| 3 | 0.56 | 1.46 | ■ | 1.46 |
| 4 | 0.50 | 1.40 | ■ | 1.40 |
| 5 | 1.22 | 2.12 | 1.22 | ■ |
| 6 | 0.66 | 1.56 | 0.66 | ■ |
| 7 | 0.92 | 1.82 | ■ | 1.82 |
| 8 | 0.44 | 1.34 | 0.44 | ■ |
| 9 | 0.46 | 1.36 | ■ | 1.36 |
| 10 | 0.62 | 1.52 | 0.62 | ■ |

*Primer Table 4.4. A black square marks an outcome that was not observed.*

This is the fundamental problem of causal inference (Lesson 09) in its clearest form. Every row has two true values, and nature reveals only one.

**The experiment's estimate.** Average the observed outcomes in each arm:

$$
\begin{aligned}
\text{treated arm (2, 3, 4, 7, 9):} &\quad \tfrac{1}{5}(1.34 + 1.46 + 1.40 + 1.82 + 1.36) = \tfrac{7.38}{5} = 1.476 \\
\text{control arm (1, 5, 6, 8, 10):} &\quad \tfrac{1}{5}(1.05 + 1.22 + 0.66 + 0.44 + 0.62) = \tfrac{3.99}{5} = 0.798 \\
\text{difference:} &\quad 1.476 - 0.798 = 0.678 \approx 0.68
\end{aligned}
$$

The true average effect is 0.9, since every student's effect is 0.9. Why does the experiment report 0.68?

**The gap is chance imbalance in baseline scores.** Every student has the same effect, so the gap cannot come from how strongly different students respond. It comes from *who* ended up in each arm. Compare the two arms on $Y_0$, the score each student would have had without the program:

- treated arm: $\tfrac{1}{5}(0.44 + 0.56 + 0.50 + 0.92 + 0.46) = \tfrac{2.88}{5} = 0.576$;
- control arm: $0.798$, as above.

By chance, the treated arm contains students with lower baseline scores, by $0.576 - 0.798 = -0.222$. So the estimate is the true effect plus this baseline difference: $0.9 - 0.222 = 0.678$. This is the split of Lesson 22, "effect among the treated plus baseline difference $B$", seen in a small sample.

Randomization does not promise that any particular sample is balanced. It promises that the assignment ignores $Y_0$ and $Y_1$, so baseline differences between the arms average out over repeated randomizations, and shrink as the sample grows (Lesson 21). As the number of students grows, the difference in observed means converges to $\mathbb{E}[Y_1 - Y_0] = 0.9$. In the language of Lesson 28, randomization cuts every arrow into $X$, the empty set satisfies the backdoor criterion, and the adjustment formula gives $\mathbb{E}[Y_x] = \mathbb{E}[Y \mid X = x]$.

What randomization does *not* provide is any individual counterfactual. Student 3's score without the program remains unobserved, exactly as Lesson 26 argued. Only the model could supply it.

## Counterfactuals in Linear Models

Without assumptions about the form of the model, a counterfactual such as $\mathbb{E}[Y_{X=x} \mid Z = z]$ may be unidentifiable even when experiments can be run. Linear models are much more forgiving.

- **Every counterfactual is identifiable once the parameters are.** The parameters fully determine the equations, and the equations determine every counterfactual through the Fundamental Law (Lesson 27). Every parameter of a linear model can be identified experimentally, through the interventional definition of direct effects (Lesson 25), so in a linear model every counterfactual is experimentally identifiable.
- **More surprisingly, the total effect is enough.** A counterfactual $\mathbb{E}[Y_{X=x} \mid Z = e]$, for any evidence $e$, is identifiable whenever the total effect $\mathbb{E}[Y \mid do(X = x)]$ is, even if some individual parameters are not. The link is:

> **Theorem 4.3.2.** Let $\tau$ be the slope of the total effect of $X$ on $Y$,
> $$\tau = \mathbb{E}[Y \mid do(x + 1)] - \mathbb{E}[Y \mid do(x)].$$
> Then for any evidence $Z = e$,
> $$\mathbb{E}[Y_{X=x} \mid Z = e] = \mathbb{E}[Y \mid Z = e] + \tau\,\big(x - \mathbb{E}[X \mid Z = e]\big). \tag{4.17}$$

**Reading the theorem.** Start with the best estimate of $Y$ given the evidence, $\mathbb{E}[Y \mid e]$. Then add the change expected in $Y$ when $X$ moves from its best estimate given the evidence, $\mathbb{E}[X \mid e]$, to the hypothetical value $x$. In a linear model that change is the same for every unit: $\tau$ per unit of $X$, the sum of products of Lesson 25. So the three steps of abduction, action and prediction collapse into one formula. The theorem's practical importance is that it answers questions about individuals, or groups of individuals, using only population data.

### Application: The Effect of Treatment on the Treated

The **effect of treatment on the treated** (ETT) is the average effect among those who actually received the treatment:

$$ETT = \mathbb{E}[Y_1 - Y_0 \mid X = 1]. \tag{4.18}$$

Apply Theorem 4.3.2 twice with the evidence $e = \{X = 1\}$, once with $x = 1$ and once with $x = 0$. Given $X = 1$, the best estimate of $X$ is $\mathbb{E}[X \mid X = 1] = 1$. So:

$$
\begin{aligned}
\mathbb{E}[Y_1 \mid X = 1] &= \mathbb{E}[Y \mid X = 1] + \tau\,(1 - 1) = \mathbb{E}[Y \mid X = 1] \\
\mathbb{E}[Y_0 \mid X = 1] &= \mathbb{E}[Y \mid X = 1] + \tau\,(0 - 1) = \mathbb{E}[Y \mid X = 1] - \tau \\
ETT &= \mathbb{E}[Y_1 \mid X = 1] - \mathbb{E}[Y_0 \mid X = 1] = \tau
\end{aligned}
$$

For Model 4.1, the total effect is the sum of products over the two directed paths: $\tau = 0.7 + 0.5 \times 0.4 = 0.9$. So $ETT = 0.9$, the same as the effect in the whole population. This holds for any evidence in a linear model: $\mathbb{E}[Y_{x+1} - Y_x \mid e] = \tau$.

### When Linearity Fails

The equality $ETT = \tau$ depends on every unit responding in the same way, which linearity guarantees. Add an interaction and it breaks. The Primer's example reverses the arrow between program and homework, so that homework affects the program, and gives the outcome an interaction term:

$$H := U_H, \qquad X := aH + U_X, \qquad Y := bX + cH + \delta XH + U_Y,$$

with independent, normally distributed errors of mean zero and $\mathrm{Var}(U_H) = \mathrm{Var}(U_X) = 1$. (Normality makes $\mathbb{E}[H \mid X]$ exactly linear in $X$, which the calculation below uses.) Now the effect of $X$ on a unit is $b + \delta H$, which depends on that unit's homework.

- **Population effect.** $\tau = b + \delta\, \mathbb{E}[H] = b$, since $\mathbb{E}[H] = 0$.
- **Effect on the treated.** $ETT = b + \delta\, \mathbb{E}[H \mid X = 1]$. The regression of $H$ on $X$ has slope $\mathrm{Cov}(X, H) / \mathrm{Var}(X) = a / (1 + a^2)$, because $\mathrm{Cov}(X, H) = a\,\mathrm{Var}(H) = a$ and $\mathrm{Var}(X) = a^2 + 1$. So $\mathbb{E}[H \mid X = 1] = a/(1 + a^2)$, and $ETT = b + \delta a/(1 + a^2)$.

The two differ by $\delta a / (1 + a^2)$. The treated units tend to be those with more homework, and with the interaction, homework changes how much the program helps. Linearity, not the experiment, was what made $ETT$ equal $\tau$. The Primer leaves this calculation as an exercise (Study question 4.3.2(c)).

## What Each Setting Can Reveal

| Setting | What it reveals about counterfactuals | What it cannot reveal |
| --- | --- | --- |
| Fully specified model | Every counterfactual, including cross-world ones (Table 4.3) | Nothing, but the model itself must be justified |
| Randomized experiment, no model | Population quantities such as $\mathbb{E}[Y_1]$ and $\mathbb{E}[Y_0]$ | Individual counterfactuals; in general, counterfactuals conditioned on evidence |
| Observational data and a graph, no functional form | Population quantities that the graph identifies (Part 3) | In general, counterfactuals conditioned on evidence |
| Linear model with the total effect identified | Every counterfactual of the form $\mathbb{E}[Y_x \mid e]$ (Theorem 4.3.2) | The guarantee is lost once interactions or other nonlinearities enter |

## Summary and Key Takeaways

1. A fully specified model defines a **synthetic population**: a complete table of observed and potential outcomes, from which any counterfactual follows by counting rows.
2. A randomized experiment reveals **one potential outcome per unit**. Without a model, the only link between an individual's counterfactuals and their observed behaviour is consistency.
3. In the Primer's ten-student trial, the estimate (0.68) differs from the true effect (0.9) because of **chance imbalance in baseline scores** ($-0.222$), not differences in response. Randomization makes such imbalance vanish on average and as samples grow.
4. **In linear models**, every counterfactual $\mathbb{E}[Y_x \mid e]$ is identified whenever the total effect is: $\mathbb{E}[Y_x \mid e] = \mathbb{E}[Y \mid e] + \tau(x - \mathbb{E}[X \mid e])$.
5. In linear models the **effect of treatment on the treated equals the population effect**. An interaction term breaks this, by $\delta a/(1 + a^2)$ in the Primer's example.

**Next step:** Lesson 30 puts the machinery to work on two practical problems: the **effect of treatment on the treated** in general, and **additive interventions**.

### Check Your Understanding

1. Fill in the row for student 7 of Table 4.3 from $U = (0.2, 0.3, 0.8)$: compute $X$, $H$, $Y$, $H_0$, $H_1$, $Y_0$ and $Y_1$, and check them against the table.
2. Suppose the experiment had instead assigned students 2, 3, 4, 7 and 9 to *control* and the rest to treatment. Compute the new estimate of the average effect, and split it into the true effect plus the baseline difference.
3. In the interaction example, explain in words why the effect of treatment on the treated exceeds the population effect when $\delta > 0$ and $a > 0$.

---

## Further Reading

*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 4, Sections 4.3.3–4.3.4 (Tables 4.3–4.4, Theorem 4.3.2, Eqs. 4.17–4.19, Study question 4.3.2).
