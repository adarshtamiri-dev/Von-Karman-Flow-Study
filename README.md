# Flow Study of Von Kármán Ogive Nose-Cones

---

**Concluded on:** 18—01—2025  
This study blends math, automation, and flow physics to explore how Von Kármán ogives behave across subsonic to hypersonic regimes.  

---

## 🎯 Objectives  
The objective of this project was to predict:  
- Drag performance  
- Shock formations  
- Flow separation  

for the same profile with Fineness Ratios – **1, 1.5, 2, 2.5, 3, 3.5, 4, and 4.5** under Mach numbers – **0.3, 0.5, 0.8, 1, 1.5, 2, 3, 4, and 5**.

---

## 🐍 Coordinate Generation with Python  
The project started with generating ogive coordinates using a Python script. This script takes inputs for:  
- Nose cone **length**  
- **Fineness ratio (L/D)**  

and returns **X-Y-Z coords** of the 2D profile.  
The ogive shape was calculated using the analytical equation for Von Kármán profiles, known for optimal supersonic drag characteristics.  
- X-axis: Axial length  
- Y-axis: Radial distance from center-line  
- Z-axis: 0 (planar profile)  

These values were then exported as a formatted `.txt` file for CAD import.  
_You can find the Python code in the repository._

---

## 🧱 CAD Construction using SolidWorks  
SolidWorks was used to construct the geometry. The point file was imported as a 2D sketch using  
**“Curve from XYZ points tool”** and revolved about the X-axis to form a smooth 3D solid of the ogive.

A surrounding flow domain was also modeled:  
- Large enough to let shocks evolve without boundary effects  
- Only a **quarter** modeled to leverage **symmetry** and reduce simulation time

---

## 🛠 Meshing in Salome  
The model was saved as `.STEP` and imported into **Salome**, an open-source CAD + mesh platform.  
Salome was used to build a clean mesh with control:

- Used **1D-2D-3D Netgen algorithm**  
- Created a refinement body near the ogive  
- Used **tetrahedrals** in domain  
- Added **prism layers** near the wall  

To handle wall effects and viscous layers:  
- Prism extrusion was used  
- Layer height, count, and growth rate tuned to match target **y+ < 1** for **k-ω SST** turbulence model  
- Smooth transition from prism to tetra mesh ensured

---

## 🔄 Export and Simulation with SimFlow  
- Grid exported as `.unv`  
- Imported into **SimFlow 5.0** (GUI for OpenFOAM)  
- SimFlow recognized boundary groups and handled conversion

### Simulation Settings:  
- Solver: `rhoCentralFoam` for compressible flow & shocks  
- Fluid: Ideal Gas (Air), γ = 1.4  
- BCs:  
  - Inlet: `supersonicInlet` (Mach, P, T specified)  
  - Outlet: `pressureOutlet`  
  - Wall: `noSlip`  
  - Planes: Symmetry  

---

## 🌪 Turbulence & Numerics  
Turbulence Model: **SST (Shear Stress Transport)**  
- Handles strong shock-boundary layer interaction  
- Blends k-ε (far-field) with k-ω (near-wall)

### Discretization:  
- 2nd-order upwind for convection  
- Linear for diffusion  
- Implicit time-stepping with fixed Courant number  
- Under-relaxation: U, P, k/ω  
- Residual targets: 1e-5 for momentum & turbulence

---

## ♻️ Parametric Workflow  
- Python script reused for different fineness ratios  
- Only needed L and L/D inputs  
- Generated new point files  
- Repeated workflow:  
  - Import → CAD → Mesh → Simulate  

This made it easy to explore many configurations quickly.

---

## 🧩 Mesh Strategies & Best Practices  
- Prism layers created manually—better wall accuracy  
- Netgen used for balance between automation and control  
- ParaView used to inspect mesh quality near nose tip

---

## ⚙️ Tools & Roles  
Each software played a specific role:  
- **Python**: Ogive generation  
- **SolidWorks**: CAD modeling  
- **Salome**: Mesh generation  
- **SimFlow**: Solver setup  
- **ParaView**: Mesh and flow field visualization

---

## 📈 Solver Performance  
- SimFlow stable even in hypersonic setups  
- `rhoCentralFoam` resilient  
- Convergence issues around Mach ~0.8 (transitional regime)  
  - Fixed using pseudo-transient terms

---

- Parametric design speeds up analysis  
- Manual control often better than full automation for accuracy  
- Pipeline was repeatable and robust

---

## 🛸 Real-World Relevance  
- Principles apply to any high-speed body:  
  - Missiles  
  - Re-entry vehicles  
- Refined mesh, smart BCs, and solver tuning are key

---

## 🔁 Iterative Improvement  
- Confidence improved with each run  
- Able to tweak mesh, solver, BCs effectively  
- Understood physical behavior through direct observation

---

**Date:** 18—01—2025
