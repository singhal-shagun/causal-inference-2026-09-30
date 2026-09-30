---
type: Lesson
title: "Lesson 07 — Potential Outcomes Intuition"
description: "The counterfactual nature of causality: two potential futures per unit, Y(1) and Y(0), whether or not we observe them."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: human:shagun-with-ai, at: 2026-07-21 }
---

# Lesson 07: Potential Outcomes Intuition

## Introduction to the Potential Outcomes Framework
While DAGs focus on causal structures, the **Potential Outcomes framework** (also known as the Neyman-Rubin model) focuses on the counterfactual nature of causality. Instead of asking "what causes what" in a graph, we ask: "What would the outcome be for a specific individual under different possible treatments?"

## Core Concept: Two Potential Futures
For any individual unit, we define two potential outcomes:
*   $Y(1)$: The outcome we would observe if the unit receives the treatment ($X=1$).
*   $Y(0)$: The outcome we would observe if the unit receives the control ($X=0$).

Crucially, these exist *even if we don't observe them*.

## The "Fundamental" Challenge
At any given time, a person can only be in one state. We can observe either $Y(1)$ OR $Y(0)$, but **never both**. 

*   If we observe the treatment ($X=1$), we observe $Y(1)$ and the outcome $Y(0)$ is the "counterfactual" (the unobserved potential outcome).
*   If we observe the control ($X=0$), we observe $Y(0)$ and $Y(1)$ is the counterfactual.

## Causal Effect at the Individual Level
The causal effect for a single individual is defined as the difference between these two potential states:
$$\text{Individual Causal Effect} = Y(1) - Y(0)$$

Since one of these is always missing, we cannot calculate the causal effect for any single individual from data alone. This is why we shift our focus to **average** causal effects in populations.

---
### Further Reading
*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 4.
