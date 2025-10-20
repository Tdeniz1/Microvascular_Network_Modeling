# Coronary Microcirculation Blood Flow Simulation

## Overview
This project models blood flow through a simplified coronary microvascular network using **Poiseuille’s law**, **Kirchhoff’s laws**, and **graph theory**.  
Multiple physiological and pathological scenarios are simulated, including:

- Healthy baseline flow  
- Mid-branch narrowing due to cholesterol buildup  
- Distal vasodilation during exercise  
- Severe distal occlusion  

The model solves for **node pressures** and **edge flows** in a multi-node vessel network and produces **shared-scale visualizations** for direct comparison across scenarios.

---------------------------------------------------------------------------------------------------------------------

## Key Features
- **Physics-based modeling**  
  Implements Poiseuille’s law with vessel-specific radii and lengths to calculate resistances.

- **Scenario analysis**  
  Four distinct physiological conditions simulated with adjusted vessel geometries.

- **Multi-scenario visualization**  
  4-panel figure with shared color scales for **flow** (edge width/color) and **pressure** (node color).

- **Quantitative outputs**  
  Prints node pressures, edge flows (μL/s), and total inflow/outflow for each scenario; includes % change vs. normal.

- **Data export**  
  Saves scenario comparison figures and per-edge flow data to CSV for further analysis.

---------------------------------------------------------------------------------------------------------------------

## Technical Approach

### Network Construction
The coronary-like vessel tree is represented as a **directed graph** (`networkx`), where:
- **Nodes** represent vessel junctions or measurement points.  
- **Edges** represent vessels with associated **length**, **radius**, and computed **resistance**.

### Flow Physics
Each vessel’s resistance is computed via Poiseuille’s law:  
`R = (8 * μ * L) / (π * r^4)`  
where `μ` is blood viscosity, `L` is length, and `r` is vessel radius.

### Pressure Solving
Kirchhoff’s current law (mass conservation) is applied at each node, and a linear system is solved for unknown pressures (`numpy.linalg.solve`).

### Flow Calculation
Edge flow is computed from pressure difference and resistance, converted to **μL/s** for readability:  
`Q_uL/s = (ΔP_Pa / R) * 1e9`

### Visualization
- **Edge width/color** → relative flow magnitude (Viridis colormap).  
- **Node color** → relative pressure (Plasma colormap).  
- **Shared scales** across scenarios enable direct visual comparison.  
- Optional **delta maps** show % change vs. normal (diverging colormap).

---------------------------------------------------------------------------------------------------------------------

## Scenarios Modeled
All four scenarios are modeled on the same coronary-like network:

1. **Normal** — Baseline geometry.  
2. **Exercise (Distal Vasodilation)** — 20% radius increase on distal microvessels.  
3. **Distal Occlusion** — 90% radius reduction on `P3 → C3`.  
4. **Cholesterol (Mid-branch Narrowing)** — 50% radius reduction on `P2 → C2`.

---------------------------------------------------------------------------------------------------------------------

## Outputs
**Console**
- Node pressures (mmHg)
- Edge flows (μL/s)
- Total inflow/outflow per scenario
- % change vs. normal totals

**Figures**
- Individual scenario plots
- 4-panel comparison figure with shared flow/pressure scales
- Optional delta maps (flow change vs. normal)

**Data**
- CSV with edge properties (length, radius, resistance, flow) per scenario

---------------------------------------------------------------------------------------------------------------------

## Example Figure

![Scenario Comparison](outputs/scenario_comparison-2.png)

The key takeaways for each of the four scenarios are shown in the figure above. The figure provides a summarized view of each pathway’s hemodynamics.In the linked .ipynb on the main branch, a more detailed quantitative analysis is available.
**Normal** – Blood flows evenly through both the upper and lower branches. Pressure drops gradually from the inlet (IN) to the outlet (OUT) without any sharp drops, reflecting uniform vessel diameters and balanced resistance across the network.

**Cholesterol (Mid-branch)** – Flow through the P2 → C2 branch is visibly reduced, and the C2 node appears warmer in color (red/orange), indicating a significant pressure drop. Other upper branches carry more of the flow. This occurs because a 50% radius reduction dramatically increases resistance in that branch (Poiseuille’s law: resistance ∝ 1/r⁴), causing a steep pressure drop before and across the stenosis. Blood preferentially routes through alternative branches with lower resistance.

**Exercise (Distal Dilation)** – Distal branches (C1, C2, C3, V1, V2) have thicker, greener edges, indicating higher flow. Pressure drops in these branches are smaller due to a 20% radius increase in the distal vessels, which significantly lowers their resistance. This promotes increased microvascular blood flow, matching the physiological need for greater oxygen delivery during exercise.

**Distal Occlusion** – The P3 → C3 → V2 branch is nearly absent (thin, dark blue edges), with C3 and V2 showing almost zero pressure. The upper branches carry most of the flow because a 90% radius reduction in P3 → C3 raises resistance so sharply that almost no blood passes through. Downstream vessels lose pressure entirely, and blood is rerouted through alternative, lower-resistance pathways, creating an uneven load distribution.

---------------------------------------------------------------------------------------------------------------------

## Technologies Used
- **Python 3.x**  
- **NumPy** — numerical computations & linear algebra  
- **NetworkX** — vascular network as a directed graph  
- **Matplotlib** — visualization & color mapping  
- **Pandas** — CSV export of results

---------------------------------------------------------------------------------------------------------------------

## Future Work

- Parameterization from patient-specific imaging (CT angiography, MRI)
- Non-Newtonian blood viscosity models
- Time-dependent simulations for pulsatile flow
- ML models to predict perfusion loss from vessel geometry


## Author

**Deniz Turk** — Biomedical Engineering student, University of South Carolina
Focused on computational modeling, data visualization, and applied physics in cardiovascular systems.
