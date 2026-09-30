---
type: Lesson
title: "Lesson 02 — Introduction to Causal Graphs"
description: "Directed Acyclic Graphs (DAGs) as visual and mathematical tools for representing assumptions about causal relationships."
resource: "/unknown/Causal Inference in Statistics_ A Primer (2016, Wiley).pdf"
tags: [causal-inference, primer, lesson]
status: draft
generated: { by: human:shagun-with-ai, at: 2026-07-19 }
---

# Lesson 02: Introduction to Causal Graphs

## The Role of Causal Graphs
In causal inference, a causal graph—also known as a Directed Acyclic Graph (DAG)—is a visual and mathematical tool to represent our assumptions about how variables in a system are related.

## Anatomy of a DAG
*   **Nodes**: Represent variables (e.g., Treatment, Outcome, Confounder).
*   **Directed Edges (Arrows)**: Represent the *direct* causal effect. An arrow from $X \to Y$ indicates that $X$ has a direct causal effect on $Y$.
*   **Acyclic**: There are no paths that start at a node and return to it by following the arrows. You cannot cause yourself through a sequence of events.

## Why Use DAGs?
1.  **Transparency**: They force us to make our assumptions explicit.
2.  **Logic**: They allow us to use rules (like the *d-separation* criterion) to determine what variables to adjust for to get an unbiased causal estimate.
3.  **Communication**: They provide a common language for domain experts, data scientists, and statisticians to talk about a causal problem.

---
### Further Reading
*Ref*: *Causal Inference in Statistics: A Primer (2016)*, Chapter 1.
