---
type: Lesson
title: "Lesson 30 — The Effect of Treatment on the Treated and Additive Interventions"
description: "Estimating ETT from observational and experimental data, the modified adjustment formula, and the effect of dose-adding policies."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: teacher/1.0, at: 2026-09-17 }
---

# Lesson 30: The Effect of Treatment on the Treated and Additive Interventions

## Where We Left Off

Lesson 29 showed what experiments and linear models can reveal about counterfactuals, and introduced the **effect of treatment on the treated** (ETT). This lesson applies the counterfactual machinery to two practical questions that the do-operator cannot even state: how much a program helps the people who *chose* to join it, and what happens when a dose is *added* to whatever each person already has.

## Application 1: Recruitment to a Program

**Example 4.4.1.** A government funds a job-training program for unemployed people. A randomized pilot shows that it works: more of those who completed it found jobs. The program is launched, and anyone who wants to can enrol. Enrolment is high, and the hiring rate among graduates is even higher than in the pilot. The program's developers ask for more funding.

Critics object. The pilot assigned people at random; the new enrolees chose to join. People who volunteer may be more resourceful and better connected, and might have found jobs anyway. What matters, the critics say, is the **benefit of the program to those who enrolled**: how much their hiring rate rose, compared with what it would have been without the training.

With $X = 1$ for training and $Y = 1$ for being hired, that quantity is the effect of treatment on the treated:

$$ETT = \mathbb{E}[Y_1 - Y_0 \mid X = 1]. \tag{4.20}$$

This is the same quantity Lesson 22 called the ATT. It has the cross-world structure of Lesson 26's freeway problem. $\mathbb{E}[Y_0 \mid X = 1]$ asks whether people who *were* trained would have found jobs *without* training, and history cannot be rerun to withhold training from them. Yet, despite this clash of worlds, the quantity can often be computed from data.

## Identifying the ETT with a Backdoor Set

Suppose a set $\mathbf{Z}$ of measured covariates satisfies the backdoor criterion for $(X, Y)$. Then:

> **Modified adjustment formula.**
> $$P(Y_x = y \mid X = x') = \sum_z P(Y = y \mid X = x, Z = z)\, P(Z = z \mid X = x'). \tag{4.21}$$

Here $x'$ is the treatment actually received and $x$ is the hypothetical one. They may differ, which is the interesting case.

**Derivation.**

$$
\begin{aligned}
P(Y_x = y \mid X = x') &= \sum_z P(Y_x = y \mid X = x', Z = z)\, P(Z = z \mid X = x') && \text{law of total probability} \\
&= \sum_z P(Y_x = y \mid X = x, Z = z)\, P(Z = z \mid X = x') && \text{Theorem 4.3.1} \\
&= \sum_z P(Y = y \mid X = x, Z = z)\, P(Z = z \mid X = x') && \text{consistency}
\end{aligned}
$$

The second step uses Theorem 4.3.1 (Lesson 28). Given $\mathbf{Z}$, the counterfactual $Y_x$ is independent of $X$, so it does not matter whether we condition on $X = x'$ or $X = x$. The third step uses consistency (Lesson 27): among units with $X = x$, the counterfactual $Y_x$ is just the observed $Y$.

**Compare it with the ordinary adjustment formula** of Lesson 19, $P(y \mid do(x)) = \sum_z P(y \mid x, z)\, P(z)$. Both condition on $\mathbf{Z}$ and average over its values. The difference is the weights. The ordinary formula weights each stratum by its share of the whole population, $P(z)$. The modified formula weights it by its share among units that received $x'$, $P(z \mid x')$. That is what a question about the treated should do: average over the kind of people who were treated.

**The ETT itself.** Apply (4.21) with $x = 0$, $x' = 1$, and use consistency for the first term, $\mathbb{E}[Y_1 \mid X = 1] = \mathbb{E}[Y \mid X = 1]$:

$$ETT = \mathbb{E}[Y \mid X = 1] - \sum_z \mathbb{E}[Y \mid X = 0, Z = z]\, P(Z = z \mid X = 1).$$

In words: the treated group's actual outcome, minus the outcome of *untreated* people with the same covariate profile as the treated.

### A Worked Example: The Drug Data of Lesson 19

Lesson 19 used Primer Table 1.1: a drug $X$, recovery $Y$, and gender $Z$, which satisfies the backdoor criterion.

| | Drug: recovered / total | No drug: recovered / total |
| --- | --- | --- |
| Men | 81 / 87 (0.931) | 234 / 270 (0.867) |
| Women | 192 / 263 (0.730) | 55 / 80 (0.688) |
| Combined | 273 / 350 (0.780) | 289 / 350 (0.826) |

**The ingredients.**

- Recovery among drug-takers: $\mathbb{E}[Y \mid X = 1] = 273/350 = 0.780$.
- Gender among drug-takers: $P(\text{man} \mid X = 1) = 87/350 = 0.249$ and $P(\text{woman} \mid X = 1) = 263/350 = 0.751$.
- Recovery without the drug, by gender: $234/270 = 0.867$ for men and $55/80 = 0.688$ for women.

**The calculation.**

$$
\begin{aligned}
\sum_z \mathbb{E}[Y \mid X = 0, z]\, P(z \mid X = 1) &= 0.867 \times 0.249 + 0.688 \times 0.751 = 0.215 + 0.517 = 0.732 \\
ETT &= 0.780 - 0.732 = 0.048
\end{aligned}
$$

Among the patients who took the drug, it raised recovery by about 4.8 percentage points. The population effect is about 5.4 points when computed from unrounded rates (Lesson 33); Lesson 19 reports 5.0, having rounded the rates to two decimals first. The two differ because the drug helps men more (about $0.931 - 0.867 = 0.064$) than women (about $0.730 - 0.688 = 0.043$), and those who chose the drug were mostly women. This is the ATT-versus-ATE difference of Lesson 22 in real numbers.

### Other Routes to the ETT

A backdoor set is not the only route. The Primer names two others, and in each case the graph tells whether the ETT can be estimated.

- **A binary treatment with both experimental and observational data.** The law of total expectation gives $\mathbb{E}[Y_0] = \mathbb{E}[Y_0 \mid X = 1]\, P(X = 1) + \mathbb{E}[Y_0 \mid X = 0]\, P(X = 0)$. By consistency, $\mathbb{E}[Y_0 \mid X = 0] = \mathbb{E}[Y \mid X = 0]$. An experiment supplies $\mathbb{E}[Y_0] = \mathbb{E}[Y \mid do(X = 0)]$, and observational data supply the rest. Solving:
  $$\mathbb{E}[Y_0 \mid X = 1] = \frac{\mathbb{E}[Y \mid do(X = 0)] - \mathbb{E}[Y \mid X = 0]\, P(X = 0)}{P(X = 1)}.$$
- **A front-door mediator**, as in Lesson 23.

## Application 2: Additive Interventions

**Example 4.4.2.** Many real interventions *add* to a variable rather than setting it. Give 5 mg/l of insulin to patients whose insulin levels already differ. Whatever caused each patient's current level keeps operating, and a fixed amount is added on top, so the differences between patients remain. The do-operator cannot describe this, because $do(x)$ switches off a variable's existing causes and sets everyone to the same value. Can the effect of such a policy be predicted from observational data, or from a trial that set $X$ to a fixed level for everyone?

Counterfactual notation makes the answer clear. Adding $q$ to a patient currently at $X = x$ produces the outcome $Y_{x+q}$. Averaged over all patients currently at $x$, that is $\mathbb{E}[Y_{x+q} \mid X = x]$: a quantity of the same form as the ETT, with the hypothetical treatment $x + q$ differing from the actual one $x$. So whenever a backdoor set exists, the modified adjustment formula (4.21) applies. Averaging over the current levels gives the effect of the policy, written $add(q)$:

$$
\begin{aligned}
\mathbb{E}[Y \mid add(q)] - \mathbb{E}[Y] &= \sum_x \mathbb{E}[Y_{x+q} \mid X = x]\, P(X = x) - \mathbb{E}[Y] \\
&= \sum_x \sum_z \mathbb{E}[Y \mid X = x + q, Z = z]\, P(Z = z \mid X = x)\, P(X = x) - \mathbb{E}[Y]
\end{aligned}
\tag{4.22}
$$

In the insulin example, $\mathbf{Z}$ might include age, weight or genetic factors, as long as they are measured and satisfy the backdoor criterion.

## Why Not Just Average the Do-Effects?

A natural shortcut would be to take the dose-response from a standard trial and average it over the population's current doses:

$$\sum_x \big( \mathbb{E}[Y \mid do(X = x + q)] - \mathbb{E}[Y \mid do(X = x)] \big)\, P(X = x).$$

This answers a different question. It describes an experiment in which people are *chosen at random* and a fraction $P(X = x)$ of them are given $x$ plus the extra dose. In the policy question, $P(X = x)$ is the share of people who reached level $x$ *by their own choices*, and such people may respond differently from people assigned to $x$. For example, people who are very sensitive to extra insulin might, given the choice, keep their levels low. In counterfactual terms:

$$\sum_x \mathbb{E}[Y_{x+q} \mid X = x]\, P(x) \;\neq\; \sum_x \mathbb{E}[Y_{x+q}]\, P(x)$$

in general. The two sides are equal when there is no confounding, that is, when $Y_x$ is independent of $X$.

> [!NOTE]
> **Science versus policy.** Pearl separates two kinds of quantity. The *scientific* one is the dose-response, $\mathbb{E}[Y \mid do(X = x + q)] - \mathbb{E}[Y \mid do(X = x)]$. It is biologically meaningful and carries over from one population to another; a laboratory can report it. The *policy* one, the effect of adding $q$ to everyone in *this* population, depends on how doses are distributed here, $P(X = x)$, and on who chose which dose. It does not carry over. A special experiment could measure the policy effect directly, but a standard trial that sets doses uniformly measures only the scientific quantity. Counterfactuals, working at the level of individual units, translate between the two.

Who asks ETT-type questions? Often the people making real decisions. A program that is already running must decide whether to keep funding it for the people who actually enrol, not for the general public. A drug that is prescribed mainly to severe cases matters most for future severe cases, not for a population average dominated by healthy people. In both, the treated were selected into treatment by characteristics related to the outcome, which is why the ETT, and not the population average, is the relevant quantity.

## The Common Signature

Both applications ask for a counterfactual whose antecedent conflicts with what was observed: $\mathbb{E}[Y_{x} \mid X = x']$ with $x \neq x'$. Both are identified by the same argument: Theorem 4.3.1 plus consistency, given a backdoor set. The applications in the next two lessons have different signatures. Attribution (Lesson 31) conditions on the outcome as well as the treatment, as in $P(Y_{x'} = y' \mid X = x, Y = y)$. Mediation (Lesson 32) uses nested counterfactuals such as $Y_{x, M_{x'}}$. Recognizing the form of the subscript expression is often the fastest way to see which kind of question is being asked and what it takes to answer it.

## Summary and Key Takeaways

1. The **effect of treatment on the treated**, $\mathbb{E}[Y_1 - Y_0 \mid X = 1]$, is the effect among those who received the treatment. It is the ATT of Lesson 22 and cannot be written with the do-operator.
2. With a backdoor set $\mathbf{Z}$, the **modified adjustment formula** $P(Y_x = y \mid x') = \sum_z P(y \mid x, z)\, P(z \mid x')$ identifies it. It differs from the ordinary adjustment formula only in weighting strata by $P(z \mid x')$ instead of $P(z)$.
3. In Lesson 19's drug data the ETT is about 0.048, against a population effect of about 0.054, because drug-takers were mostly women, for whom the drug helps less.
4. For a binary treatment, the ETT can also be computed from experimental and observational data combined, or through a front-door mediator.
5. **Additive interventions** are ETT-type questions. Averaging do-effects over current doses answers a different, random-assignment question.
6. The dose-response is a transportable scientific quantity; the effect of a dose-adding policy depends on the population. Counterfactuals translate between them.

**Next step:** Lesson 31 turns to **attribution**: the probabilities of causation used in legal liability and personal decisions.

### Check Your Understanding

1. In the derivation of (4.21), the second step replaces $X = x'$ by $X = x$ inside a conditional probability. Why would the same replacement be invalid without conditioning on $\mathbf{Z}$?
2. In the drug example, compute the effect of treatment on the *untreated*, $\mathbb{E}[Y_1 - Y_0 \mid X = 0]$, using (4.21) with the roles of $x$ and $x'$ swapped. Is it larger or smaller than the ETT, and why?
3. Give a policy question of your own that is an ETT question rather than an ATE question, and say who "the treated" are.

---

## Further Reading

*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 4, Sections 4.4.1–4.4.2 (Eqs. 4.20–4.22, Study question 4.4.1) and Table 1.1.
