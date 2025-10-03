# Sensor Placement Optimisation for AI-Informed Radiant Heating Control (Swansea Bay Net-Zero)

## 🎯 Project Overview

This repository documents a critical comparative study on optimising the placement of directional Passive Infrared (PIR) sensors. The project is a foundational element for developing an **AI-informed heating control system** that drives maximal energy efficiency in modern buildings.

The goal is to determine the most effective and efficient optimisation algorithm (Differential Evolution vs. Bayesian Optimisation) for maximising sensor coverage in complex, obstructed spaces.

The successful implementation of this research directly supports **Swansea Bay's Net-Zero Drive** by enabling highly granular, predictive control over advanced radiant heating systems, significantly curtailing energy wastage.

## ⚙️ Methodology: Optimisation Algorithm Comparison

We compare two leading global optimisation techniques—Differential Evolution (DE) and Bayesian Optimisation (BO)—using an **identical, highly realistic objective function**. This ensures a robust, like-for-like benchmark.

### 1. The Objective Function (The Sensor Model)

The objective function simulates a real-world sensor network, representing the performance of a **self-powered IoT wireless sensor mesh**. This mesh is the data source for the wider AI control system. The function seeks to **MAXIMISE the total covered floor area** by minimising the negative coverage score.

| Component | Description | Relevance to Reality |
| :--- | :--- | :--- |
| **Data Source** | **Self-Powered IoT Wireless Sensor Mesh (PIR)** | Models the low-power, maintenance-free sensor network reporting occupancy and activity data. |
| **Room Dimensions** | Fixed 10m x 8m floor plan. | Matches typical commercial office spaces in the UK. |
| **Sensor Type** | Directional PIR sensors (5 units). | Models the actual hardware being deployed. |
| **Parameters** | 15 total parameters: $(x, y)$ position and $\text{orientation}(\theta)$ for each sensor. | Enables the optimisation of directional coverage. |
| **Obstacle Blocking** | Ray-casting checks for Line-of-Sight (LoS) blockage by two fixed cabinet obstacles. | **Crucial** for avoiding "dead zones" behind furniture where occupancy might be missed. |

---

### 2. Optimisation Algorithms Benchmarked

| Algorithm | Method | Total Budget (Function Calls) | Key Feature |
| :--- | :--- | :--- | :--- |
| **Differential Evolution (DE)** | Population-based stochastic search (`scipy.optimize`) | 9000 ($300 \text{ iter} \times 30 \text{ pop}$) | Robust global exploration. |
| **Bayesian Optimisation (BO)** | Model-based search (`scikit-optimize`) | 9000 (Identical to DE) | **Sample-efficient** and intelligent search using a Gaussian Process. |

---

## 🔬 Rationale for Comparison & Net-Zero Value

### The AI-Informed Heating System

This optimisation work is the crucial first step for a next-generation system that links high-quality occupancy data with advanced radiant heating.

1.  **AI-Informed Prediction:** The AI takes the reliable data from the optimised sensor mesh to not only detect current occupancy but also to **predict future activity categories** (e.g., meeting about to start, single person working, extended vacancy).
2.  **Radiant Heating Control:** This predictive insight then informs the control of the low-inertia **underfloor, under wall, and under ceiling heating elements**.

### The Net-Zero Challenge of Radiant Heating

Radiant heating offers superior comfort and energy efficiency but suffers from **thermal inertia** (it takes time to heat up and cool down). This inertia is a major challenge for energy saving, as heating an empty room wastes energy.

#### The Solution: Predictive Sensor Optimisation

By selecting the superior optimisation algorithm, we guarantee the best possible sensor placement, leading to **high-confidence data for the AI**. This high-quality, predictive data allows the system to:

1.  **Pre-Heat Accurately:** Initiate heating *just in time* for predicted occupancy, mitigating the thermal inertia delay without wasting energy on long standby periods.
2.  **Shut-Down Proactively:** Switch off the radiant elements *before* a predicted vacancy, capitalising on the residual heat and saving the maximum energy possible.

**The Net-Zero Impact:** The integration of accurate sensor placement, self-powered IoT, and AI-informed control over cutting-edge radiant heating creates a system capable of operating with near-perfect efficiency. This shift to **predictive, demand-led HVAC control** is a major contribution towards reducing the operational carbon footprint of commercial buildings across the Swansea Bay City Region, accelerating the drive to net-zero.

---

## 💡 Future-Proofing: Handling Complex Geometries

While the current benchmark uses a rectangular room for foundational comparison, the script's design allows for seamless extension to real-world, non-square layouts. This ensures the research is directly applicable to modern commercial buildings, which frequently feature irregular floor plans (e.g., L-shaped, T-shaped, or rooms with angled walls).

### Adaptation Strategy:

1.  **Define Complex Boundary:** The current simple `is_inside(x, y)` check would be replaced. The room would be defined as a **polygon** (a list of vertices).
2.  **Point-in-Polygon Filter:** A robust geometric algorithm (such as ray-casting or the winding number method) would be integrated into the `Room` class.
3.  **Refined Evaluation:** The objective function would then use this **Point-in-Polygon test** to precisely filter the underlying coverage grid, ensuring that only cells *truly inside* the complex room geometry (and not inside obstacles) are counted towards the total available coverage area.

This modular approach guarantees that the core optimisation logic and the sensor model remain consistent, allowing the focus to remain on which algorithm handles the increased geometric complexity best.

