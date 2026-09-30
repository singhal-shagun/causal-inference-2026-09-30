---
type: Lesson
title: "Lesson 16 — Connecting DAGs and Potential Outcomes"
description: "How the structural (DAG) and potential outcomes frameworks translate into one another, and the assumptions — positivity above all — that no graph can express."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson, synthesis]
status: draft
generated: { by: human:shagun-with-ai, at: 2026-07-23 }
---

# Lesson 16: Connecting DAGs and Potential Outcomes

## Two Sides of the Same Coin

Having completed our exploration of the identification assumptions (SUTVA, Exchangeability, and Positivity), we can now look back and synthesize how the **Structural (DAG)** and **Potential Outcomes** frameworks complement one another.

These are not competing models; they are two sides of the same coin. But the correspondence is not total, and the places where it breaks down are as instructive as the places where it holds.

---

## The Translation Key

We can map features of a structural causal graph to assumptions in potential outcomes:

| DAG Structural Concept | Potential Outcomes Equivalence | Action |
| :--- | :--- | :--- |
| **No open back-door paths** | Unconditional Exchangeability: $(Y(1), Y(0)) \perp\!\!\!\perp X$ | No adjustment needed. |
| **Back-door paths blocked by $\mathbf{Z}$** | Conditional Exchangeability: $(Y(1), Y(0)) \perp\!\!\!\perp X \mid \mathbf{Z}$ | Adjust for $\mathbf{Z}$. |
| **No cross-unit arrows; each node one intervention** | SUTVA: No Interference and Consistency | Draw one graph that stands for any unit; define each treatment level singularly. |
| ***No counterpart*** | Positivity: $0 < P(X=1 \mid \mathbf{Z}=z) < 1$ | Cannot be read from the graph — tabulate the data. |

The first row is stated as "no **open** back-door paths" deliberately. A back-door path that runs through a collider is already blocked without any conditioning, so its mere existence does not break unconditional exchangeability. What matters is whether a back-door path is open, not whether one can be drawn.

The fourth row is the one to dwell on, and the next section takes it up.

> [!NOTE]
> **How do the SUTVA requirements show up in the graph?**
>
> A DAG is only as good as its **completeness**: it is assumed to be a true map of all causal dependencies among the variables drawn, with no omitted connections. That completeness assumption is what underwrites the *exchangeability* rows above — an omitted common cause is precisely an unblocked back-door path you failed to draw.
>
> SUTVA is a different matter, and is often folded into "no missing arrows" too loosely. Its two requirements constrain the graph in two *different* ways, and only the first is genuinely about arrows:
>
> 1. **No Interference — a constraint on arrows *between* units.**
>    * Written out honestly, a study of $n$ units needs a node for *every* unit's treatment and *every* unit's outcome. If units interfere — patient $i$'s assignment $X_i$ affecting patient $u$'s recovery $Y_u$ — the graph also needs arrows crossing from one unit to another:
>
>    ```text
>      WITH INTERFERENCE                 WITHOUT INTERFERENCE
>      ----------------------------      ----------------------------
>      X1 --> Y1                         X1 --> Y1
>         \--> Y2                        X2 --> Y2
>      X2 --> Y2                         X3 --> Y3
>         \--> Y1                         ...
>      X3 --> Y3
>       ...                              n copies, identical and
>                                        sealed off from each other
>      2n nodes, cross-linked; this
>      picture is tied to THIS study     so ONE copy stands for all:
>      of THIS size
>                                              Z --> X --> Y
>    ```
>
>    * This is the sense in which "draw one graph that stands for any unit" **is** the no-interference assumption, not a drafting convenience. Deleting the cross-unit arrows leaves $n$ subgraphs that are identical and isolated. *Because* they are identical, any one of them represents the rest, and the compact three-node DAG we have been drawing since Lesson 02 becomes available. Allow a single cross-unit arrow back in and the copies stop being identical, no one of them speaks for the others, and nothing short of the full $2n$-node graph is honest.
>    * Note also what the abstraction costs when it fails: the one-unit graph is not merely inconvenient to fix, it is *unavailable*. This is the graphical mirror of the algebraic point in Lesson 14 — under interference $Y_u$ is a function of the whole assignment vector $(x_1, \ldots, x_n)$, so there is no single $Y_u(1)$ to draw an arrow into. Same assumption, two notations.
> 2. **Consistency — a constraint on what a *node means*.**
>    * This one is not about a missing arrow at all. It requires that the node $X$ denote a single, well-defined intervention. If $X=1$ silently covers a brand-name drug for some patients and a degraded generic for others, the graph has one node where it needs two, and $Y(1)$ names no single potential outcome.
>    * Note the contrast with confounding: a hidden variable pointing to **both** $X$ and $Y$ is an omitted common cause, which breaks *exchangeability*, not consistency. Consistency is broken by an ambiguous node, not by an absent one.
>
> In short: no-interference says the arrows you see are all the arrows there are; consistency says the nodes you see each mean exactly one thing.

---

## Why Both Frameworks Exist

1. **DAGs are excellent for Design**: They allow us to visually check which paths are open or closed, identify colliders, and find a valid adjustment set $\mathbf{Z}$ using simple graph rules such as d-separation. (Note that *causal discovery* is a distinct and more ambitious activity — inferring the graph itself from data — and is not what the graph is doing for us here. Here the graph is an input, asserted from domain knowledge.)
2. **Potential Outcomes are excellent for Algebra**: Once the graph has told us that exchangeability holds given $\mathbf{Z}$, potential outcomes provide the mathematical vocabulary to write estimands, perform stratification, and calculate causal effects such as the $\text{ATE}$.

> [!IMPORTANT]
> **Complementary Strengths:**
>
> * Causal graphs answer **"What variables do I need to control for?"**
> * Potential outcomes answer **"How do I mathematically calculate the treatment effect once I control for those variables?"**

---

## What the Graph Cannot Tell You

A DAG encodes *which variables depend on which* — a qualitative structure of arrows. Positivity is a statement of an entirely different type: it concerns the **support** of the observed distribution, meaning whether both treatment arms are actually populated within each stratum. Nothing about the shape of a graph records how many units landed where.

The consequence is that two studies with **identical DAGs** can differ completely on positivity. Consider the over-65 prescription rule from Lesson 13:

$$P(X=1 \mid Z = 70) = 1, \qquad P(X=1 \mid Z = 60) = 0$$

Its DAG is $Z \to X$, $Z \to Y$, $X \to Y$ — an utterly ordinary confounding triangle. Apply the back-door criterion and the graph returns a clean verdict: the back-door path $X \leftarrow Z \to Y$ is blocked by $Z$, so adjust for $Z$ and the effect is identified. The graph is correct about everything it is capable of seeing, and the study is still hopeless, because no value of $Z$ contains both treated and untreated patients. A second study with the same three arrows and a 50/50 split in every age stratum would be perfectly analysable. The graphs would be indistinguishable.

This is the graphical face of the point Lesson 13 made algebraically: exchangeability is about the **absence of bias**, positivity about the **presence of information**. D-separation detects the first and is blind to the second.

### Where Each Assumption Is Actually Checked

| Assumption | Visible in the DAG? | Where it is really settled |
| :--- | :--- | :--- |
| **Exchangeability** | **Yes** — via d-separation and the back-door criterion | Read off the graph, *given* the graph is right |
| **No Interference** | **Partly** — in how the graph is drawn (one unit, no cross-unit arrows) | A modelling decision, justified by domain knowledge |
| **Consistency** | **No** — concerns what a node means, not how it connects | The definition of the treatment variable |
| **Positivity** | **No** — concerns the support of the data | Tabulate $P(X=1 \mid \mathbf{Z}=z)$ in the sample |

> [!CAUTION]
> The back-door criterion gives you a promise **with a condition attached**. Keep the promise and the condition apart:
>
> * **What the DAG does tell you:** adjusting for $\mathbf{Z}$ removes confounding, so exchangeability holds. *Provided* every stratum of $\mathbf{Z}$ also contains both treated and untreated units (positivity condition), the adjustment formula would identify the effect.
> * **What the DAG does not tell you:** whether positivity actually holds true for your data. The graph cannot confirm positivity, because it has no record of how many units fell into each stratum.
>
> So check positivity separately in every study: count the treated and untreated units in each stratum of $\mathbf{Z}$.

---

## Summary and Key Takeaways

1. **Two Languages, One Subject:** DAGs and potential outcomes are not rival theories. The graph says *which* variables to adjust for; potential outcomes say *how* to compute the effect once you have them.
2. **The Translation Holds for Exchangeability:** No open back-door path corresponds to unconditional exchangeability; back-door paths blocked by $\mathbf{Z}$ correspond to conditional exchangeability. This is the part of the dictionary that is exact.
3. **"Open" Matters:** A back-door path through a collider is blocked already. Existence of a back-door path is not the criterion; *openness* is.
4. **SUTVA Constrains the Graph in Two Ways:** No Interference forbids cross-unit arrows and is what allows a single-unit graph at all. Consistency requires each node to denote one intervention — a constraint on meaning, not on connectivity.
5. **Consistency Is Not Confounding:** An omitted common cause of $X$ and $Y$ breaks exchangeability. An ambiguous treatment node breaks consistency. These are different failures with different remedies.
6. **Positivity Has No Graphical Counterpart:** Two studies with identical DAGs can differ entirely on overlap. D-separation cannot see an empty stratum, so positivity must be checked against the data every time.
7. **Next Step:** Lesson 17 goes beneath the graph to **Structural Causal Models**, which replace the qualitative arrows with explicit equations and make interventions a matter of substituting one equation for another.

---

### Further Reading

*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 4.
