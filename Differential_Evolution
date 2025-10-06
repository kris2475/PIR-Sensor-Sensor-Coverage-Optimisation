# Differential Evolution (DE): Finding the Best Layout for PIR Sensors

This document explains the **Differential Evolution (DE)** algorithm—a clever method of trial-and-error—and shows how it is used to solve the tricky problem of getting the most coverage from your **PIR (Passive Infrared) sensors** in a building, based on the approach in the provided repository.

---

## 1. Introduction to Differential Evolution (DE)

Differential Evolution is a widely used and robust **metaheuristic**.  
Think of a metaheuristic as a clever, broad strategy designed to find a very good answer to a difficult problem—especially when pure mathematics or simple searching would take too long or simply wouldn't work.

- **Why DE?**  
  DE is particularly useful for complex layout problems, such as finding the best spot to put sensors, because it doesn't need smooth, easy-to-calculate functions.  
  Instead of relying on calculus, DE uses **differences between potential solutions** to create new, better ideas for sensor placement.

---

### 🎯 Analogy: Searching for the Best Treasure Map

- **The Map Coordinates (Solution Vector \(x\))**  
  A list of sensor settings. In the analogy, it’s a map with coordinates and angles showing exactly where to place and point every metal detector.

- **The Score (Fitness Function)**  
  The percentage of the field covered by the detectors. The higher the score, the better the layout.

---

### The Optimisation Problem Explained Simply

The aim:  
**Find the perfect spots and directions for PIR sensors so they cover the largest possible area of a room or building.**

- **The Score (Fitness Function):**  
  How much of the room is covered by the sensors. Higher is better.

- **The Recipe (Solution Vector \(x\)):**  
  A list of instructions for all sensors. For \(N\) sensors, each includes:
  - Side-to-side position (\(x\))
  - Up-and-down position (\(y\))
  - The angle (\(\theta\)) the sensor is facing

---

## 2. How the DE Algorithm Works: Four Simple Steps

DE improves a **population of solutions** over several generations. Each cycle consists of:

---

### A. Initialisation (Starting the Search)

- Start with a **population (NP)** of random sensor layouts.  
- Each layout respects the room boundaries.

📌 **Analogy:**  
Treasure hunters drop 50 random flags (layouts). Each flag represents sensor positions and angles inside the field.

---

### B. Mutation (Generating a New Idea)

For each layout (\(x_{i,G}\)), create a **mutant vector** (\(v_{i,G}\)) using three random layouts (\(r_1, r_2, r_3\)):

\[
v_{i,G} = x_{r_1,G} + F \cdot (x_{r_2,G} - x_{r_3,G})
\]

- \(x_{r_1,G}\): Base vector  
- \(F\): Differential weight (scaling factor)  
- \((x_{r_2,G} - x_{r_3,G})\): The “nudge” applied

📌 **Analogy:**  
A hunter takes one base map and shifts its sensor positions using the difference between two other maps. The result is a **Mutant Map**.

---

### C. Crossover (Mixing and Blending)

Form a **trial vector** (\(u_{i,G}\)) by mixing parameters from the **mutant vector** (\(v_{i,G}\)) and the **original vector** (\(x_{i,G}\)).

- Controlled by the **crossover rate (CR)**, usually high (≈80%).  
- Ensures at least one component comes from the mutant.

📌 **Analogy:**  
A hunter blends the new Mutant Map with their original. Some parts are kept, others replaced.

---

### D. Selection (Survival of the Fittest)

Compare fitness of the original (\(x_{i,G}\)) vs. trial (\(u_{i,G}\)):

\[
x_{i,G+1} =
\begin{cases} 
u_{i,G}, & \text{if } f(u_{i,G}) \geq f(x_{i,G}) \\
x_{i,G}, & \text{otherwise}
\end{cases}
\]

📌 **Analogy:**  
The hunters keep whichever map covers more ground. The worse one is discarded.

---

➡️ Repeat steps **Mutation → Crossover → Selection** for many generations until improvement stalls.

---

## 3. Specifics for Finding the Best Sensor Spots

For PIR sensor optimisation, fitness depends on **coverage**.

---

### A. Fitness Calculation (Coverage Metric)

1. **Room Representation:**  
   Convert the room into a 2D grid of pixels.

2. **Sensor Model:**  
   Each PIR sensor has a fan-shaped coverage area (range + angle).

3. **Counting:**  
   Determine which grid cells are covered by at least one sensor.

4. **Fitness Score:**

\[
\text{Fitness}(x) = \frac{\text{Number of covered points}}{\text{Total number of points in the area}}
\]

---

### B. Why DE is Effective Here

- DE **avoids local optima** (e.g., clustering sensors in one corner).  
- Mutation + Crossover explore a broad set of possibilities.  
- Over time, DE converges toward the **best coverage layout**.

---

## ✅ Summary

Differential Evolution is an **evolution-inspired trial-and-error strategy**.  
For PIR sensor placement, it:

- Starts with random sensor layouts
- Mutates and blends them into new candidates
- Selects the fittest layouts each generation
- Iteratively improves coverage until an optimal setup is found

This makes DE a powerful method for **maximising sensor coverage in real-world environments**.

---
