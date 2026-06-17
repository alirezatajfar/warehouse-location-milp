# warehouse-location-milp
Uncapacitated facility location problem solved with MILP (PuLP/CBC) — minimises fixed warehouse opening costs plus demand-weighted shipping costs across 25 customers and 8 candidate sites.

# Warehouse Location Problem — MILP

A Python implementation of the **uncapacitated facility location problem (UFLP)** using Mixed-Integer Linear Programming (MILP).  
Solved with [PuLP](https://coin-or.github.io/PuLP/) and the open-source CBC solver.

---

## Problem Description

Given:
- A set of **customers**, each with a known demand and a location on a 2D plane
- A set of **candidate warehouse locations**, each with a fixed opening cost

Decide:
1. **Which warehouses to open** (binary: open / closed)
2. **Which open warehouse serves each customer** (binary assignment)

**Objective:** Minimise total cost = Σ fixed opening costs + Σ (demand × Euclidean distance) shipping costs

This is a classic combinatorial optimisation problem in supply chain and logistics, with applications in distribution network design, public facility planning, and last-mile delivery.

---

## Model Formulation

| Symbol | Meaning |
|--------|---------|
| `i ∈ I` | Set of customers |
| `j ∈ J` | Set of candidate warehouses |
| `d_i` | Demand of customer `i` |
| `f_j` | Fixed opening cost of warehouse `j` |
| `c_ij` | Shipping cost: Euclidean distance between `i` and `j` |
| `y_j ∈ {0,1}` | 1 if warehouse `j` is opened |
| `x_ij ∈ {0,1}` | 1 if customer `i` is assigned to warehouse `j` |

**Objective:**

```
min  Σ_j f_j · y_j  +  Σ_i Σ_j d_i · c_ij · x_ij
```

**Constraints:**

```
Σ_j x_ij = 1        ∀ i ∈ I     (each customer served by exactly one warehouse)
x_ij ≤ y_j          ∀ i, j      (assignment only to open warehouses)
y_j, x_ij ∈ {0, 1}              (binary variables)
```

---

## Instance (randomly generated)

| Parameter | Value |
|-----------|-------|
| Customers | 25 |
| Candidate warehouses | 8 |
| Grid size | 100 × 100 |
| Demand per customer | Uniform [10, 30] |
| Fixed cost per warehouse | Uniform [1 000, 5 000] |
| Shipping cost | Demand × Euclidean distance |

A random seed (`np.random.seed(42)`) is set for reproducibility.

---

## Output

The notebook prints the optimal cost breakdown and produces a visualisation saved as `warehouse_solution.png`:

- ⭐ **Coloured stars** — open warehouses  
- ✗ **Grey crosses** — closed candidate locations  
- **Dots** — customers, coloured by their assigned warehouse  
- **Lines** — customer-to-warehouse assignment paths  

---

## Requirements

```
python >= 3.8
pulp
numpy
matplotlib
```

Install with:

```bash
pip install pulp numpy matplotlib
```

---

## Running the Notebook

```bash
jupyter notebook warehouse_location.ipynb
```

Or open it in [VS Code](https://code.visualstudio.com/) with the Jupyter extension.  
No Google account or Colab session required — runs fully locally.

---

## Project Context

Built as a coursework exercise in **Operations Research / Industrial Engineering**.  
The model structure (objective, constraints, variable types) is standard UFLP and can be extended with:
- Capacity constraints on warehouses
- Multiple product types
- Real geographic coordinates and road distances
