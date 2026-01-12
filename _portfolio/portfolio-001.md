---
title: "Numerical Linear Algebra Lab: Direct vs. Iterative Solvers for a Two-Point BVP"
excerpt: "A MATLAB-based study benchmarking direct solvers, stationary iterations, and Krylov methods on a discretized two-point boundary value problem (with varying ε to stress conditioning). Includes accuracy/time tables, convergence curves, and practical implementation details.<br/><a href='https://zhouyang0694.github.io/files/数值线性代数上机报告.pdf' target='_blank' rel='noopener'>⬇ Download the full report (PDF)</a>"
collection: portfolio
date: 2025-05-29
---

## Overview

This report is a hands-on numerical linear algebra project focused on **solving the linear systems that arise from discretizing a two-point boundary value problem (BVP)**. The goal is not only to obtain a correct numerical solution, but also to **compare algorithms from multiple angles**:

- **Accuracy** (residuals and error vs. the analytical solution)
- **Efficiency** (runtime and iteration counts)
- **Stability & sensitivity** (how performance changes as ε varies)

The experiments are designed to be *algorithm-comparative*: each solver is applied under the same settings, and results are summarized with tables and figures for quick inspection.

---

## Problem Setup

A two-point BVP is discretized into a linear system:

- Grid size: **n = 100**
- Parameter sweep: **ε = 1, 0.1, 0.01, 0.0001**
- Outputs include:
  - Infinity-norm residual \\( \|b - Ax\|_\infty \\)
  - Error vs. exact solution (MSE / Max Error)
  - Runtime / iteration behavior

This setup naturally produces matrices with structure (notably **tridiagonal form** in key steps), enabling both general-purpose and structure-exploiting solvers.

---

## Methods Implemented

### 1) Direct Methods (One-shot solves)

- **Gaussian elimination (no pivoting)**  
- **Cholesky decomposition** (via normal equations in some comparisons)
- **Thomas algorithm (tridiagonal solver)** — exploits matrix structure for speed

### 2) Stationary Iterative Methods

- **Jacobi**
- **Gauss–Seidel**
- **SOR (Successive Over-Relaxation)**  
  - Includes exploration of **relaxation factor ω** and its effect on convergence

### 3) Krylov Subspace Methods

- **Steepest Descent** (included for completeness; observed to be a poor fit in this setting)
- **Conjugate Gradient (CG)**  
  - Applied via normal equations in cases where SPD requirements are needed

---

## Experimental Highlights (What the reader will see)

### Direct-method benchmark (accuracy + speed)

The report compares the direct methods across ε values and shows that:

- All direct methods achieve **very small residuals** (near machine precision in many cases).
- The **Thomas algorithm** is dramatically faster when the system is tridiagonal, while matching accuracy.

### Iterative-method benchmark (convergence behavior)

The iterative section provides:

- Side-by-side tables of residuals, MSE / max error, **iteration counts**, and runtime.
- Visual plots of solution curves (numerical vs. exact) and **convergence curves** over iterations.
- A practical discussion of why some methods may stall or require careful parameter tuning.

A particularly useful part is the **SOR analysis**:
- The report visualizes how the **spectral radius** of the SOR iteration matrix changes with ω,
  giving intuition for selecting ω and understanding when SOR actually accelerates convergence.

### Key takeaways discussed in the analysis

- Why **solver residuals can be tiny** while the solution error vs. exact solution is dominated by **model/discretization error**
- How **ε influences conditioning**, iteration counts, and practical runtime
- When a “theoretically strong” method (e.g., steepest descent) can still be a poor engineering choice
- Why exploiting structure (tridiagonal → Thomas) is often the biggest win

---

## Implementation Notes

The appendix includes key MATLAB implementations (with sanity checks and error handling), making this report both:

- a reproducible experiment log, and
- a compact reference for classic solvers (direct + iterative) in practice.

---

## Download

- **PDF report:** [数值线性代数上机报告.pdf](https://zhouyang0694.github.io/files/数值线性代数上机报告.pdf)

