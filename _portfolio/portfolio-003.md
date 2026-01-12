---
title: "Real-Root Finding Portfolio: Polynomials & Smooth Functions with Chebyshev Techniques"
excerpt: "A MATLAB-focused numerical analysis portfolio on **finding all real roots** of polynomials and smooth functions. The work targets two core challenges—**reliable root counting** and **effective interval localization**—and benchmarks multiple classic/modern strategies (companion vs. colleague eigenvalue solvers, Chebyshev–Sturm isolation, and Boyd–Battles Chebyshev proxy polynomials) on ill-conditioned test cases such as multiple roots and near-colliding root clusters.<br/><a href='https://zhouyang0694.github.io/files/多项式和光滑函数零点问题的多实根求解算法.pdf' target='_blank' rel='noopener'>⬇ Download the full report (PDF)</a>"
collection: portfolio
date: 2025-12-18
---

## What this portfolio contains

This report is a **comparative algorithm study + implementation write-up** for computing **all real zeros** in two settings:

1) **Polynomials (real-root extraction as the primary goal)**  
2) **General smooth functions on a given interval (root-finding via Chebyshev approximation)**

The emphasis is not only “getting roots”, but **understanding numerical stability** and **failure modes** (multiple roots, clustered roots, oscillatory/decaying functions), and translating that understanding into practical implementation choices.

---

## Algorithms implemented & compared

### 1) Eigenvalue-based polynomial root solvers
- **Companion-matrix eigenvalue method (monomial basis):** transforms polynomial root-finding into an eigenvalue problem, then filters real roots by an imaginary-part threshold.
- **Colleague-matrix eigenvalue method (Chebyshev basis):** repeats the same eigenvalue philosophy but in an **orthogonal polynomial basis**, aiming for improved stability through scaling and Chebyshev representation.

**Why it matters:** The study highlights how “mathematically equivalent” formulations can behave very differently in floating-point arithmetic—especially for ill-conditioned polynomials.

### 2) Chebyshev–Sturm method (root counting + isolation + refinement)
- Builds a **Sturm-sequence sign-change counting** framework in the Chebyshev basis and uses recursive bisection to isolate real roots.
- Once an interval contains a single root, the root is refined (e.g., bisection / Newton) until the interval is below a target tolerance.

**Key strength:** When real roots are **simple and well-separated**, Chebyshev–Sturm typically achieves very high accuracy because it “separates first, refines later”.

### 3) Boyd–Battles method for smooth functions (Chebfun-style workflow)
- On a specified interval, the function is **adaptively interpolated** in Chebyshev points.
- Uses **standardChop**-style plateau detection on Chebyshev coefficients to choose a truncation order and form a **Chebyshev proxy polynomial**.
- Roots of the proxy polynomial are obtained via the **colleague matrix**, then refined by **Newton iterations** (with residual checks using the original function).
- Includes practical engineering details: when proxy degree grows too large, the interval is **recursively split** to keep the polynomial degree manageable.

**Key caveat:** This approach is intrinsically *interval-based*; proxy-polynomial roots outside the target interval are not considered reliable.

### 4) (Extension) Homotopy continuation framework (appendix)
- Provides a **total-degree homotopy** continuation template for solving multivariate polynomial systems, extending the discussion beyond univariate root finding.

---

## Benchmark design & diagnostic outputs

### Polynomial test suite (covers “nice” and “nasty” cases)
Representative cases include:
- Polynomials with **mostly complex roots** but a small number of real roots (testing real-root filtering logic),
- The **Wilkinson polynomial** (classic ill-conditioning for coefficients/root sensitivity),
- **Multiple-root** examples,
- **Near-colliding root clusters** where spacing approaches floating-point resolution.

Diagnostics include:
- Absolute error statistics (max/mean),
- Plots showing error by root index (useful for spotting which roots are hardest),
- Root-count mismatches when algorithms fail to resolve clustered roots.

### Smooth-function test suite (interval root enumeration)
Includes:
- A mixed oscillatory function on a bounded interval,
- An oscillatory **exponentially decaying** function (tests approximation quality and root capture),
- A **near-multiple-root** function (tests conditioning limits even when the method “finds two roots”).

Diagnostics include:
- Root locations with residual checks `|f(r)|`,
- Function plots with root markers (global view + zoomed view near difficult regions).

---

## Main takeaways (what a reader should learn)
- **Root counting and root isolation are “global” tasks**; relying on local single-start iterations is not enough when the goal is *all* roots.
- **Orthogonal bases + scaling (Chebyshev/colleague)** can substantially improve stability compared with monomial/companion approaches on ill-conditioned problems.
- **Multiple roots and extremely close roots are fundamentally hard in double precision**—even strong methods may lose digits, conflate two roots into one, or miss roots when counting becomes unreliable.
- For smooth functions, **Boyd–Battles is powerful when roots are separated and the function is well-approximated**, but it inherits the same conditioning limitations in near-multiple-root regimes and requires a user-supplied search interval.

---

## Skills demonstrated
- Numerical linear algebra: eigenvalue-based root finding via structured companion/colleague linearizations  
- Polynomial bases & stability: monomial vs. Chebyshev representations, scaling strategies  
- Verified enumeration mindset: root counting + interval isolation + refinement pipeline  
- Practical scientific computing: adaptive approximation, heuristics (standardChop), diagnostics and visualization-driven evaluation  
- Algorithmic extension: homotopy continuation framing for polynomial systems

---

## Downloads
- **PDF report:** [多项式和光滑函数零点问题的多实根求解算法.pdf](https://zhouyang0694.github.io/files/多项式和光滑函数零点问题的多实根求解算法.pdf)

