# Sensor Placement Optimisation for Smart Building Net-Zero Initiative (Swansea Bay)

## 🎯 Project Overview

This repository documents a comparative study on optimising the placement of directional Passive Infrared (PIR) sensors within indoor environments. The primary goal is to determine the most effective and efficient optimisation algorithm for maximising sensor coverage in complex, obstructed spaces.

The outcome of this research directly supports **Swansea Bay's Net-Zero Drive** by improving the efficacy and reliability of **Room Occupancy and Activity Detection** systems. Accurate, high-fidelity detection is crucial for intelligent, demand-driven control of Heating, Ventilation, and Air Conditioning (HVAC) systems, which will significantly reduce energy wastage.

## ⚙️ Methodology: Optimisation Algorithm Comparison

We compare two leading global optimisation techniques—Differential Evolution (DE) and Bayesian Optimisation (BO)—using an **identical, highly realistic objective function**. This ensures a robust, like-for-like benchmark.

### 1. The Objective Function (The Sensor Model)

The objective function simulates a real-world sensor network and is deliberately complex to test the algorithms' robustness. It seeks to **MAXIMISE the total covered floor area** (the number of non-obstacle grid cells covered) by minimising the negative coverage score.

| Component | Description | Relevance to Reality |
| :--- | :--- | :--- |
| **Room Dimensions** | Fixed 10m x 8m floor plan. | Matches typical commercial office spaces in the UK. |
| **Sensor Type** | Directional PIR sensors (5 units). | Models the actual hardware being deployed. |
| **Parameters** | 15 total parameters: $(x, y)$ position and $\text{orientation}(\theta)$ for each sensor. | Enables the optimisation of directional coverage. |
| **Obstacle Blocking** | Ray-casting checks for Line-of-Sight (LoS) blockage by two fixed cabinet obstacles. | **Crucial** for identifying "dead zones" behind furniture. |
| **FOV Constraint** | Fixed 100° Field of View and 5.0m detection range. | Directly incorporates sensor hardware specifications. |

---

### 2. Optimisation Algorithm A: Differential Evolution (DE)

The DE script utilises a population-based, stochastic search strategy, which is well-known for its ability to escape local minima. It is robust and computationally straightforward.

| Parameter | Value | Rationale |
| :--- | :--- | :--- |
| **Library** | `scipy.optimize.differential_evolution` | A standard, reliable implementation in Python. |
| **Strategy** | `best1bin` | A typically effective strategy for multi-modal optimisation. |
| **Total Budget** | 9000 Function Calls | $300 \text{ iterations} \times 30 \text{ population size}$. **Sets the benchmark limit.** |
| **Search Space** | 15 Dimensions (as defined above). | Defines the placement and orientation constraints. |

### 3. Optimisation Algorithm B: Bayesian Optimisation (BO)

The BO script, employing a Gaussian Process surrogate model and the Expected Improvement acquisition function, is engineered for **sample efficiency**. It builds a probabilistic model of the objective function to intelligently focus its search on the most promising areas, aiming to achieve a superior result with fewer costly function evaluations than DE.

| Parameter | Value | Rationale |
| :--- | :--- | :--- |
| **Library** | `scikit-optimize` (`skopt`) | A robust BO framework. |
| **Surrogate Model** | Gaussian Process (Default) | Highly effective for modelling complex, expensive objective functions. |
| **Acquisition Function** | Expected Improvement (`EI`) | Cleverly balances 'exploration' (searching unknown areas) and 'exploitation' (refining known good areas). |
| **Total Budget** | 9000 Function Calls | **Identical to DE** to ensure a direct and fair performance comparison. |

---

## 🔬 Rationale for Comparison & Net-Zero Value

### Why Compare DE and BO?

The sensor placement problem is highly multi-modal and high-dimensional (15 parameters). We must select the most computationally efficient algorithm for scalability across numerous rooms in large commercial buildings.

1.  **DE** offers robust global exploration but its search process is blind to previous results outside the current population.
2.  **BO** is designed to be more **sample-efficient**, learning from every past evaluation to inform the next best location to test.

By benchmarking both with the same budget, we aim to determine which approach:
* **Optimises faster:** Which algorithm discovers a better coverage solution earlier in the process?
* **Achieves higher final quality:** Which algorithm yields the highest maximum coverage score after 9000 function evaluations?

The selected, superior algorithm will then be deployed within the final planning software package.

### Contribution to Swansea Bay's Net-Zero Drive

This work is a fundamental component of the strategy to achieve net-zero carbon emissions across the region's commercial and public infrastructure.

#### The Problem: Inefficient HVAC

HVAC systems typically account for a colossal amount of energy consumption in commercial buildings. Systems often run based on conservative schedules or simple temperature probes, resulting in huge energy wastage by:

* Heating, cooling, or lighting rooms that are completely unoccupied.
* Running ventilation at peak levels when activity is low.

#### The Solution: High-Efficacy Occupancy Data

By using the optimal sensor placement determined by the winning optimisation algorithm, we guarantee the highest possible room coverage. This, in turn, ensures:

1.  **Reliable Occupancy Data:** The system reliably and accurately knows when a space is truly empty.
2.  **Granular Activity Detection:** Superior coverage provides more nuanced insights into activity levels and movement patterns.

**The Net-Zero Impact:** This high-quality data allows the HVAC control system to move from wasteful time-based schedules to **real-time, spatial demand-based control**. This switch provides immediate, verifiable energy savings and directly supports the reduction of operational carbon emissions, helping the Swansea Bay region meet its ambitious net-zero targets.
