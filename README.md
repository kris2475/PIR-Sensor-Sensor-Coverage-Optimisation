🚀 Sensor Placement Optimisation for AI-Informed Radiant Heating Control (Swansea Bay Net-Zero)
🎯 Project Overview
This repository documents a critical comparative study on optimising the placement of directional Passive Infrared (PIR) sensors. The project is a foundational element for developing an AI-informed heating control system that drives maximal energy efficiency in modern buildings.

The goal was to determine the most effective and efficient optimisation algorithm (Differential Evolution vs. Bayesian Optimisation) for maximising sensor coverage in complex, obstructed spaces while ensuring line-of-sight (LoS) constraints are respected.

The successful implementation of this research directly supports Swansea Bay's Net-Zero Drive by enabling highly granular, predictive control over advanced radiant heating systems, significantly curtailing energy wastage.

⚙️ Methodology: Optimisation Algorithm Benchmark & Results
I compared two leading global optimisation techniques—Differential Evolution (DE) and Bayesian Optimisation (BO)—using an identical, highly realistic objective function. This rigorous approach ensured a robust benchmark focusing on solution quality and crucial computational cost.

Both algorithms include ray–obstacle checks, preventing PIR cones from unrealistically “seeing through walls.”

1. The Objective Function (The Sensor Model)
The objective function simulates a real-world sensor network, representing the performance of a self-powered IoT wireless sensor mesh. The function seeks to MAXIMISE the total covered floor area by minimizing the negative coverage score.

Component	Description	Relevance to Reality
Data Source	Self-Powered IoT Wireless Sensor Mesh (PIR)	Models the low-power, maintenance-free sensor network reporting occupancy and activity data.
Room Dimensions	Fixed 10m x 8m floor plan, evaluated on a 1.0m grid.	Balances computational speed with spatial resolution.
Sensor Type	Directional PIR sensors (5 units).	Models the actual hardware being deployed.
Parameters	15 total parameters: (x, y) position and orientation(Theta) for each sensor.	Enables the optimisation of directional coverage in 15 dimensions.
Obstacle Blocking	Ray-casting checks for Line-of-Sight (LoS) blockage by two fixed cabinet obstacles, using a 0.5m step size.	Crucial for avoiding "dead zones" behind furniture where occupancy might be missed.

Export to Sheets
2. Optimisation Algorithms Benchmarked and Conclusion
We used a practical, reduced budget to directly test the wall-clock efficiency of the algorithms.

Algorithm	Method	Budget (Function Calls)	Observed Wall-Clock Runtime	Practical Verdict
Differential Evolution (DE)	Population-based stochastic search (scipy.optimize)	≈420	Minutes	FEASIBLE
Bayesian Optimisation (BO)	Model-based search (scikit-optimize)	≈450	Hours	IMPRACTICAL

Export to Sheets
🛑 Analysis of Efficiency: Why Bayesian Optimisation Failed
While BO is designed for sample efficiency (fewer calls to the objective function), the high dimensionality of the problem (15 parameters) meant that the overhead of fitting and updating the Gaussian Process model was disproportionately expensive.

The total optimization time is roughly calculated as: Time_Total = (N_calls * Time_Objective) + Time_ModelUpdate.

The Time of Model Update component in BO, dedicated to fitting the complex Gaussian Process (GP) model and optimizing the acquisition function, dominated the total runtime. Differential Evolution (DE), with its simpler vector operations, proved to be the superior and only practical solution.

🔬 Rationale for DE Selection & Net-Zero Value
The computational efficiency and proven robustness of Differential Evolution allows us to guarantee the best possible sensor placement within a practical time frame, leading to high-confidence data for the AI.

The AI-Informed Heating System
This optimisation work is the crucial first step for a next-generation system that links high-quality occupancy data with advanced radiant heating.

AI-Informed Prediction: The AI takes the reliable data from the optimised sensor mesh to not only detect current occupancy but also to predict future activity categories (e.g., meeting about to start, single person working, extended vacancy).

Radiant Heating Control: This predictive insight then informs the control of the low-inertia underfloor, under wall, and under ceiling heating elements.

The Net-Zero Challenge of Radiant Heating
Radiant heating offers superior comfort and energy efficiency but suffers from thermal inertia (it takes time to heat up and cool down). This inertia is a major challenge for energy saving, as heating an empty room wastes energy.

The Solution: Predictive Sensor Optimisation
By selecting the superior optimisation algorithm (DE), we guarantee the best possible sensor placement. This high-quality, predictive data allows the system to:

Pre-Heat Accurately: Initiate heating just in time for predicted occupancy, mitigating the thermal inertia delay without wasting energy on long standby periods.

Shut-Down Proactively: Switch off the radiant elements before a predicted vacancy, capitalising on the residual heat and saving the maximum energy possible.

The Net-Zero Impact: The integration of accurate sensor placement, self-powered IoT, and AI-informed control over cutting-edge radiant heating creates a system capable of operating with near-perfect efficiency. This shift to predictive, demand-led HVAC control is a major contribution towards reducing the operational carbon footprint of commercial buildings across the Swansea Bay City Region, accelerating the drive to net-zero.

💡 Future-Proofing: Handling Complex Geometries
While the current benchmark uses a rectangular room for foundational comparison, the script's design allows for seamless extension to real-world, non-square layouts. This ensures the research is directly applicable to modern commercial buildings, which frequently feature irregular floor plans (e.g., L-shaped, T-shaped, or rooms with angled walls).

Adaptation Strategy:
Define Complex Boundary: The current simple is_inside(x, y) check would be replaced. The room would be defined as a polygon (a list of vertices).

Point-in-Polygon Filter: A robust geometric algorithm (such as ray-casting or the winding number method) would be integrated into the Room class.

Refined Evaluation: The objective function would then use this Point-in-Polygon test to precisely filter the underlying coverage grid, ensuring that only cells truly inside the complex room geometry (and not inside obstacles) are counted towards the total available coverage area.

This modular approach guarantees that the core optimisation logic, now proven practical with DE, can handle the increased geometric complexity.


