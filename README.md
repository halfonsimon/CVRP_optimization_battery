# Battery Swap Routing Optimization (CVRP)

Comparing three algorithmic approaches to solve the **Capacitated Vehicle Routing Problem (CVRP)** applied to e-scooter battery collection — from brute-force enumeration to production-ready optimization.

---

## The Problem

A fleet of collection trucks must visit every e-scooter in a city to swap depleted batteries. Each scooter is visited exactly once, each truck has a capacity limit, and the goal is to **minimize total distance traveled**.

This is an NP-hard combinatorial optimization problem. The gap between a naive approach and a smart one is the difference between milliseconds and 24,000 years.

---

## Approaches

### `Enumeration_solver.ipynb` — Brute Force
Tests every possible route combination exhaustively.
- **Guarantees the optimal solution**
- Works well up to ~6 scooters
- Exponential time complexity — becomes completely impractical at scale

| Scooters | Combinations | Time |
|----------|-------------|------|
| 4 | ~1,600 | 20ms |
| 8 | ~4,000,000 | 54s |
| 15 | ~62 trillion | **24,000 years** |

### `OR-Tools_Solver.ipynb` — Google OR-Tools
Production-grade solver using constraint propagation and guided local search.
- **Handles 1,000+ scooters in under 30 seconds**
- Uses the same MILP formulation with binary variables x^k_ij
- Configurable between small demo mode and large-scale mode

---

## Formulation

The problem is modeled as a **Mixed-Integer Linear Program (MILP)**:

- **Variables**: Binary x^k_ij — truck k travels from node i to node j
- **Objective**: Minimize total distance ∑∑∑ d_ij · x^k_ij
- **Constraints**: Each scooter visited exactly once, capacity limits respected, flow conservation enforced, subtour elimination

---

## Setup

```bash
pip install ortools matplotlib numpy
```

Open either notebook in Jupyter and run cells sequentially. Problem parameters (number of scooters, trucks, capacity) are all in **Cell 2** — easy to swap between demo and large-scale configurations.

---

## Results

The OR-Tools solver finds a high-quality solution for 1,000 scooters across 20 trucks in under 30 seconds — a problem instance where brute force would take longer than the age of the universe.

---

*Academic project — Simon Halfon & Sinai Abbou | Reichman University*
