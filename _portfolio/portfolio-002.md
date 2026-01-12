---
title: "Optimization Algorithms Benchmark: From Unconstrained to Constrained Problems"
excerpt: "A MATLAB-based benchmarking portfolio that compares classic unconstrained and constrained optimization methods on Rosenbrock-type test problems, with line-search/globalization strategies and detailed visual diagnostics.<br/><a href='https://zhouyang0694.github.io/files/优化实验报告_实验一.pdf' target='_blank' rel='noopener'>⬇ Download Experiment I Report (PDF)</a>&emsp;<a href='https://zhouyang0694.github.io/files/优化实验报告_实验二.pdf' target='_blank' rel='noopener'>⬇ Download Experiment II Report (PDF)</a>"
collection: portfolio
date: 2025-12-08
---

## Overview
This portfolio consolidates two optimization lab reports into one end-to-end experimental story: **starting from unconstrained optimization** (how different search directions behave on nonconvex valleys), and extending to **nonlinear constrained optimization** (how algorithms enforce feasibility while approaching KKT points).

Across both parts, I focused on:
- **Algorithmic implementation details** (direction generation, globalization, numerical stability tricks)
- **Fair comparison protocols** (multiple initializations, unified stopping rules, comparable metrics)
- **Interpretability** via **optimization trajectories** and **log-scale convergence curves**

---

## What I implemented
### Part A — Unconstrained Optimization (Experiment I)
Implemented and benchmarked the following classic methods:
- **Gradient Descent**
- **Newton’s Method** (with safeguards when Hessian is singular/indefinite)
- **BFGS (Quasi-Newton)**
- **Nonlinear Conjugate Gradient** (FR & PRP variants, sensitivity noted)
- **Trust-Region Method** (model-based robustness)

Testbeds:
- **modified_rosenbrock_2d** (Rosenbrock + sinusoidal ripples → many local minima)
- **rosenbrock_4d** (higher-dimensional curved valley → highlights scalability & conditioning issues)

Globalization / step size:
- **0.618 (Golden-section) “approximate exact” line search**
- **Armijo–Wolfe (strong Wolfe) inexact line search**

**Key takeaway:** Newton/BFGS/Trust-Region generally show much faster local convergence than Gradient Descent, while Conjugate Gradient can be highly sensitive to line-search quality and may require restart strategies.

---

### Part B — Constrained Optimization (Experiment II)
Implemented and compared two constrained solvers:
- **Augmented Lagrangian Method (ALM)**  
  - Outer loop: penalty + multiplier updates  
  - Inner loop: quasi-Newton (BFGS) minimization of the augmented Lagrangian
- **Sequential Quadratic Programming (SQP)**  
  - Per-iteration QP subproblem (local quadratic model + linearized constraints)  
  - Merit function + line search for globalization

Testbeds:
- **2D constrained modified Rosenbrock** with nonlinear equality + nonlinear inequality constraints  
  - emphasizes **nonconvex feasible manifold** and **multiple attraction basins**
- **4D constrained Rosenbrock** with linear equality + mixed inequalities  
  - emphasizes **higher dimension** and **active/inactive constraint behavior**

Stopping rule:
- Unified **KKT-residual-based criterion** (stationarity + feasibility + complementarity), threshold at **1e-6**.

Globalization / line search:
- **Golden-section** vs **Armijo–Wolfe** (same philosophy as Part A, for consistent comparison)

**Key takeaway:** both ALM and SQP are robust under multiple initial points, but they exhibit different “personalities”:
- **SQP** typically drops error faster near the solution (Newton-type local behavior) and tends to move along the feasible set sooner.
- **ALM** often shows a staged pattern: **first feasibility**, then **objective reduction**, depending on penalty/multiplier updates.

---

## Experimental design & metrics
To keep comparisons meaningful, I used:
- **Multiple initial points** to test robustness and sensitivity to attraction basins
- **Log-scale convergence plots** (e.g., \|∇f\| + \|c\| or KKT residual) to visualize regimes
- For constrained problems: recorded **objective value**, **feasibility violation**, **KKT residual**, and iteration counts
- For 2D cases: plotted **contours + feasible set + trajectories** to interpret search behavior geometrically

---

## Highlights / insights
- **Line search matters as much as the algorithm.**  
  In practice, Armijo–Wolfe tends to be more stable than a “pseudo exact” golden-section approach when the 1D function along the search direction is not unimodal.
- **Nonconvexity + multi-start is unavoidable.**  
  On sinusoid-perturbed Rosenbrock landscapes, different initial points and globalization strategies can converge to different local minima.
- **Constraint handling changes the geometry of progress.**  
  SQP enforces constraint linearization in each step (often hugging feasibility), whereas ALM balances feasibility via penalty/multiplier dynamics.

---

## Tech stack
- **MATLAB** for core optimization implementations (directions, updates, constraint Jacobians, QP interface where needed)
- **Python** for automated experiment pipelines and visualization (paths / convergence curves), when applicable
- Reproducible structure: separate **problem definitions**, **line search module**, **solver module**, and **logging/plotting**

---

## Downloads
- **Experiment I (Unconstrained Optimization Report, PDF):**  
  [优化实验报告_实验一.pdf](https://zhouyang0694.github.io/files/优化实验报告_实验一.pdf)

- **Experiment II (Constrained Optimization Report, PDF):**  
  [优化实验报告_实验二.pdf](https://zhouyang0694.github.io/files/优化实验报告_实验二.pdf)

