---
type: Lesson
title: "Lesson 08 — Potential Outcomes Notation and Individual Effects"
description: "Formalizing potential outcomes for a unit u and defining the Individual Causal Effect."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: human:shagun-with-ai, at: 2026-07-21 }
---

# Lesson 08: Potential Outcomes Notation and Individual Effects

## Formalizing the Framework
In the Potential Outcomes framework, we focus on what an individual unit would experience under different treatments. We call these the two potential states of the world for a unit $u$.

<details>
<summary>What is the difference between the notation used for individual causal effect in previous lesson vs this lesson?</summary>

There is no difference in the definition; Lesson 08 formalizes the intuition provided in Lesson 07 into the standard mathematical notation used in the Potential Outcomes framework.

**Comparison of the two lessons:**

| Feature | Lesson 07 (Intuition) | Lesson 08 (Notation) |
| :--- | :--- | :--- |
| **Concept** | Introduces the **Idea** of potential outcomes. | Formalizes the **Mathematical Notation**. |
| **ICE Definition** | Describes it as the difference between two potential states. | Defines it as $\text{ICE}_u = Y_u(1) - Y_u(0)$. |
| **Missing Data** | Describes it as a "challenge" or "counterfactual." | Formally defines the observed outcome $Y_u$ as a function of the treatment and unobserved potential states. |

**Why they are separate:**
1.  **Lesson 07** is focused on the **Conceptual Leap**: It introduces the core problem—that we can define potential futures ($Y(1), Y(0)$) but can only ever observe one of them. It aims to build the intuition that causal inference is about reconstructing those unobserved futures.
2.  **Lesson 08** is focused on the **Standardized Notation**: It bridges the concept to the rigorous notation required for causal proofs and statistical operations. It introduces the "Consistency" formula: $Y_u = X_u Y_u(1) + (1 - X_u) Y_u(0)$, which is the backbone for calculating population-level effects (like the Average Causal Effect) in later lessons.

In short, **Lesson 07 is the story, and Lesson 08 is the formal language.** 
</details>

## Notation 
* **$X_u$**: The treatment assigned to unit $u$ ($1$ for treated, $0$ for control).
* **$Y_u(1)$**: The potential outcome for unit $u$ if they receive treatment ($X=1$).
* **$Y_u(0)$**: The potential outcome for unit $u$ if they receive control ($X=0$).

The **observed outcome**, $Y_u$, is then a defined mathematical function of the treatment assignment and the potential outcomes:
$$Y_u = X_u Y_u(1) + (1 - X_u) Y_u(0)$$

## Individual Causal Effect
The **Individual Causal Effect** ($ICE$) for unit $u$ is the direct comparison of these two potential states:
$$\text{ICE}_u = Y_u(1) - Y_u(0)$$

### Intuition: The Counterfactual Contrast
The term $\text{ICE}_u = Y_u(1) - Y_u(0)$ represents a *counterfactual contrast*. It requires us to imagine the state of the world that *didn't* happen. If we observe a patient who received the drug ($X=1$) and recovered ($Y=1$), the causal effect is the difference between this reality and a hypothetical reality where that same patient did not get the drug.

## The Fundamental Challenge
As established in Lesson 07, $Y_u(1)$ and $Y_u(0)$ cannot both be observed for the same unit $u$. We define the unobserved outcome as the **counterfactual**.

> [!CAUTION]
> The fundamental problem of causal inference is that we can only calculate the $\text{ICE}_u$ for an individual if we have knowledge of the counterfactual. Because $Y_u(1)$ or $Y_u(0)$ is always missing, we are forced to shift our focus from individual effects to population-level **Average Causal Effects (ACE)**.
