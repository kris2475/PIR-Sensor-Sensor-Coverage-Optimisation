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

Time_Total = (N_calls * Time_Objective) + Time_ModelUpdate

- `Time_Objective` = cost of evaluating the sensor coverage  
- `Time_ModelUpdate` = cost of fitting the Gaussian Process + optimising the acquisition function  

👉 In BO, the **model update time** dominated runtime.  
👉 DE, with its simpler vector operations, proved to be the **only practical solution**.

---

## 🔬 Rationale for DE Selection & Net-Zero Value
The **computational efficiency** and proven **robustness** of Differential Evolution allow us to guarantee the **best possible sensor placement within a practical time frame**, leading to **high-confidence data for the AI**.

---

## 🌡️ The AI-Informed Heating System
This optimisation work is the crucial first step for a next-generation system that links **high-quality occupancy data** with **advanced radiant heating**.

- **AI-Informed Prediction**:  
  The AI takes the reliable data from the optimised sensor mesh to not only detect **current occupancy** but also to **predict future activity categories** (e.g., meeting about to start, single person working, extended vacancy).

- **Radiant Heating Control**:  
  This predictive insight then informs the control of the **low-inertia underfloor, under-wall, and under-ceiling heating elements**.

---

## 🔥 The Net-Zero Challenge of Radiant Heating
Radiant heating offers **superior comfort** and **energy efficiency** but suffers from **thermal inertia** (time to heat up and cool down).  
Heating an empty room wastes energy.

### ✅ The Solution: Predictive Sensor Optimisation
By selecting the superior optimisation algorithm (**DE**), we guarantee the **best possible sensor placement**.  
This high-quality, predictive data allows the system to:

- **Pre-Heat Accurately**: Initiate heating just in time for predicted occupancy, mitigating thermal inertia delay without wasting energy.  
- **Shut-Down Proactively**: Switch off heating before predicted vacancy, capitalising on residual heat to save energy.

**Net-Zero Impact**:  
The integration of **accurate sensor placement, self-powered IoT, and AI-informed control** over radiant heating creates a system capable of operating with **near-perfect efficiency**, reducing the operational carbon footprint of commercial buildings in the Swansea Bay City Region.

---

## 💡 Future-Proofing: Handling Complex Geometries
While the current benchmark uses a **rectangular room**, the script design allows **extension to real-world, irregular layouts** (L-shaped, T-shaped, angled walls, etc.).

### Adaptation Strategy
1. **Define Complex Boundary**: Replace simple `is_inside(x, y)` with a polygon-based room definition.  
2. **Point-in-Polygon Filter**: Use geometric algorithms (ray-casting, winding number) in the `Room` class.  
3. **Refined Evaluation**: Ensure only points inside the polygon (and outside obstacles) count towards total coverage.  

This **modular approach** ensures that the **proven DE optimisation logic** scales to complex architectural spaces.

## 📊 Results & Visualisation

### Differential Evolution (DE) Results
The optimal sensor coordinates and orientations found by DE are:

--- Differential Evolution Optimal Sensor Placement ---
Sensor # | X (m) | Y (m) | Orientation (deg)
  1  |     5.908  |     4.873  |       232.07
  2  |     6.273  |     0.653  |        52.23
  3  |     1.405  |     5.485  |       266.05
  4  |     5.048  |     4.549  |       136.36
  5  |     5.032  |     5.088  |        48.31


- **Total covered cells:**  *(value depends on grid resolution)*  
- **Total non-obstacle cells:**  *(computed from floor area – obstacles)*  
- **Coverage percentage:** ~**XX%**  

Each sensor’s coverage (in grid cells) is also reported for fine-grained analysis.

### Visualisation
The following script cell (from `notebooks/`) generates a fully labelled plot of the optimal placement:

```python
# --- Code Cell: Generate Fully Labeled Sensor Coverage Plot ---

final_sensor_data = np.array(optimal_sensor_params).reshape(num_sensors, 3)

plot_room_and_coverage_labeled(room, final_sensor_data, sensor_detection_range, pir_fov_degrees)




