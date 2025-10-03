# 🚀 Sensor Placement Optimisation for AI-Informed Radiant Heating Control (Swansea Bay Net-Zero)

## 🎯 Project Overview
This repository documents a critical comparative study on optimising the placement of directional Passive Infrared (PIR) sensors. The project is a foundational element for developing an AI-informed heating control system that drives maximal energy efficiency in modern buildings.

The goal was to determine the most effective and efficient optimisation algorithm (**Differential Evolution vs. Bayesian Optimisation**) for maximising sensor coverage in complex, obstructed spaces while ensuring line-of-sight (LoS) constraints are respected.

The successful implementation of this research directly supports **Swansea Bay's Net-Zero Drive** by enabling highly granular, predictive control over advanced radiant heating systems, significantly curtailing energy wastage.

---

## ⚙️ Methodology: Optimisation Algorithm Benchmark & Results
I compared two leading global optimisation techniques—**Differential Evolution (DE)** and **Bayesian Optimisation (BO)**—using an identical, highly realistic objective function.  
This rigorous approach ensured a robust benchmark focusing on **solution quality** and crucial **computational cost**.

Both algorithms include **ray–obstacle checks**, preventing PIR cones from unrealistically “seeing through walls.”

---

### 1. The Objective Function (The Sensor Model)
The objective function simulates a real-world sensor network, representing the performance of a self-powered IoT wireless sensor mesh.  
The function seeks to **MAXIMISE the total covered floor area** by minimizing the negative coverage score.

| Component         | Description                                                   | Relevance to Reality                                                                 |
|-------------------|---------------------------------------------------------------|--------------------------------------------------------------------------------------|
| **Data Source**   | Self-Powered IoT Wireless Sensor Mesh (PIR)                   | Models the low-power, maintenance-free sensor network reporting occupancy & activity |
| **Room Dimensions** | Fixed 10m x 8m floor plan, evaluated on a 1.0m grid.         | Balances computational speed with spatial resolution                                 |
| **Sensor Type**   | Directional PIR sensors (5 units).                            | Models the actual hardware being deployed                                            |
| **Parameters**    | 15 total parameters: (x, y, orientation) for each sensor.     | Enables the optimisation of directional coverage in 15 dimensions                    |
| **Obstacle Blocking** | Ray-casting checks for LoS blockage by 2 fixed cabinet obstacles | Crucial for avoiding “dead zones” behind furniture                                   |

---

### 2. Optimisation Algorithms Benchmarked and Conclusion
We used a **practical, reduced budget** to directly test the wall-clock efficiency of the algorithms.

| Algorithm                | Method                                        | Budget (Function Calls) | Observed Runtime | Verdict      |
|---------------------------|-----------------------------------------------|--------------------------|------------------|--------------|
| **Differential Evolution (DE)** | Population-based stochastic search (`scipy.optimize`) | ≈420                    | Minutes          | ✅ FEASIBLE   |
| **Bayesian Optimisation (BO)**  | Model-based search (`scikit-optimize`)             | ≈450                    | Hours            | ❌ IMPRACTICAL |

---

## 🛑 Analysis of Efficiency: Why Bayesian Optimisation Failed
While BO is designed for **sample efficiency** (fewer calls to the objective function), the **high dimensionality** of the problem (15 parameters) meant that the overhead of fitting and updating the **Gaussian Process model** was disproportionately expensive.

**Total optimisation time:**


