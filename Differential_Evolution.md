# Differential Evolution (DE): Finding the Best Layout for PIR Sensors

This document explains the **Differential Evolution (DE)** algorithm — a trial-and-error optimisation method — and shows how it is used to maximise coverage from **PIR (Passive Infrared)** sensors in a room or building.

---

## 1. Introduction to Differential Evolution (DE)

Differential Evolution is a robust **metaheuristic** useful for hard optimisation problems (like sensor-layout design) that do not have smooth or easy-to-differentiate objective functions.

DE uses the **differences between candidate solutions** to guide the search instead of gradient-based calculus.

### Analogy: Searching for the Best Treasure Map

- **Solution vector** \(\mathbf{x}\) — the list of sensor parameters (positions and angles).
- **Fitness** — the percentage (or ratio) of the room covered by the sensors; higher is better.

For \(N\) sensors the solution (recipe) might be represented as:

$$
\mathbf{x} = [x_1, y_1, \theta_1,\; x_2, y_2, \theta_2,\; \dots,\; x_N, y_N, \theta_N ].
$$

---

## 2. How the DE Algorithm Works: Four Simple Steps

DE evolves a **population** of candidate layouts over generations.

### A. Initialisation
Generate a population of \(NP\) random, valid sensor layouts within the room boundaries.

---

### B. Mutation

For each target vector \(\mathbf{x}_{i,G}\) (individual \(i\) in generation \(G\)) construct a mutant vector \(\mathbf{v}_{i,G}\) by combining three other randomly chosen population members \(r_1,r_2,r_3\) (all indices different from \(i\)):

$$
\mathbf{v}_{i,G} = \mathbf{x}_{r_1,G} + F \cdot \bigl(\mathbf{x}_{r_2,G} - \mathbf{x}_{r_3,G}\bigr),
$$

where \(F\) is the differential weight (a scalar usually in \([0,2]\), commonly ~0.5–0.9).

---

### C. Crossover

Create a trial vector \(\mathbf{u}_{i,G}\) by mixing components from \(\mathbf{v}_{i,G}\) and the target \(\mathbf{x}_{i,G}\) according to the crossover rate \(CR\). The usual practice ensures at least one component is taken from the mutant.

---

### D. Selection (Survival of the Fittest)

Compare fitness of the trial vector \(\mathbf{u}_{i,G}\) versus the original \(\mathbf{x}_{i,G}\). The winner moves to the next generation:

$$
\mathbf{x}_{i,G+1} =
\begin{cases}
\mathbf{u}_{i,G}, & \text{if } f(\mathbf{u}_{i,G}) \ge f(\mathbf{x}_{i,G}) \\
\mathbf{x}_{i,G}, & \text{otherwise}
\end{cases}
$$

Repeat mutation → crossover → selection for many generations until no meaningful improvement occurs.

---

## 3. Specifics for Finding the Best Sensor Spots

### A. Fitness Calculation (Coverage Metric)

1. **Room grid:** discretise the room into a 2D grid of small cells (pixels).
2. **Sensor model:** model each PIR sensor as a fan-shaped region (range, angle, orientation).
3. **Coverage counting:** compute which grid cells are covered by at least one sensor.
4. **Fitness:**

$$
\mathrm{Fitness}(\mathbf{x}) = \frac{\text{Number of covered points}}{\text{Total number of points in the area}}.
$$

---

### B. Why DE is Effective

- Uses vector differences to explore widely (reduces risk of getting stuck in local optima).
- Combines exploration (mutation) and exploitation (selection) efficiently.
- Good balance of global search and local refinement for sensor layout problems.

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

## ℹ️ Notes on LaTeX in GitHub

- Use `$ ... $` for inline math, and `$$ ... $$` for display math blocks.
- Always leave a blank line before and after `$$ ... $$` for proper rendering.
- Inside `cases`, use `\\` for line breaks (as shown above).
- Equations will render on GitHub, but if viewing in plain text (like some editors), you may see raw LaTeX.


