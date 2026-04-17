<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0D1117,30:0EA5E9,70:7C3AED,100:F59E0B&height=180&section=header&text=BioPhys%20Nexus&fontSize=50&fontColor=FFFFFF&animation=twinkling&fontAlignY=35&desc=Cloud-Native%20Molecular%20Dynamics%20%26%20Protein%20Refinement&descSize=16&descColor=0EA5E9&descAlignY=58" width="100%" alt="BioPhys Nexus Header"/>

[![Modal Deploy](https://img.shields.io/badge/Backend-Modal%20A10G-7C3AED?style=for-the-badge&logo=nvidia&logoColor=white)](https://modal.com)
[![Vercel](https://img.shields.io/badge/Frontend-Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://vercel.com)
[![OpenMM](https://img.shields.io/badge/Engine-OpenMM%208-0EA5E9?style=for-the-badge&logo=moleculer&logoColor=white)](https://openmm.org)
[![License](https://img.shields.io/badge/License-MIT-16A34A?style=for-the-badge)](LICENSE)

**A production-grade bioinformatics platform that transforms raw AI-predicted protein structures into docking-ready conformations using GPU-accelerated molecular dynamics.**

[Live Demo](https://biophys-refinement-4gj5w26hx-hassanahmed2has-projects.vercel.app) · [Engineering Docs](docs/ENGINEERING_JOURNEY.md) · [Report Bug](https://github.com/HassanAhmed2Ha/biophys-refinement-lab/issues)

---

</div>

## 🌟 The Scientific Mandate: Refining The Gold Standard

The advent of generative AI architectures (e.g., ESMFold, AlphaFold 3) fundamentally accelerated structural biology. However, our systematic validation revealed a terrifying anomaly: **The AlphaFold Hallucinations**. Even the world's most advanced AI occasionally hallucinates severe physical impossibilities—frozen topologies harboring cataclysmic steric clashes and colossal internal energy spikes ($>50,000$ kJ/mol).

**BioPhys Nexus** was engineered to shatter this ice. We do not merely fix flawed models; we enforce rigorous thermodynamic interrogation upon the world's best AI. By transitioning from static generational models into **Dynamic Physical Environments**, we force these hallucinated topologies down to their true biological energy minima, yielding structurally flawless, **Docking-Ready** cellular analogs.

> *"We are building the Motherboard that makes the AI Processors useful for real-world Drug Discovery."*

---

## 🖥️ Platform Telemetry & Visual Analytics

<p align="center">
  <img src="https://raw.githubusercontent.com/HassanAhmed2Ha/biophys-refinement-lab/main/dashboard_metrics.png" alt="BioPhys Nexus Dashboard" width="90%" style="border-radius: 15px; box-shadow: 0 0 20px rgba(14, 165, 233, 0.2);"/>
</p>

| Metric | Scientific Context |
| :--- | :--- |
| **Interactive 3D Trajectory** | Real-time WebGL rendering of the `.dcd` physical trajectory. Watch the protein reach thermodynamic equilibrium live in the browser. |
| **Live Thermodynamics** | Tracks massive Negative Potential Energy stabilization offsets (e.g., `-87,294` kJ/mol drops) indicating successful clash resolution. |
| **Dynamic Salinity Control** | Researchers can adjust Molar concentration (e.g., physiological `0.15M NaCl`) directly from the UI, updating the explicit solvent matrix instantly. |

---

## 🔬 Core Physics Engine: The "Time That Is Not Time"

Traditional Energy Minimization behaves merely as a "Photograph". Structural reality, however, is a chaotic, fluid dance governed entirely by relentless heat and microscopic time. We orchestrate four absolute computational laws natively in `core/minimization.py`:

1. **The Explicit Solvent Matrix (TIP3P):** Proteins fold exclusively in response to the Hydrophobic Effect. We explicitly construct symmetric bounding limits violently flooded with thousands of real `TIP3P` water molecules and native `0.15 M (Na+/Cl-)` salinity to neutralize highly electronegative topological bonds.
2. **The Thermal Dynamo (Langevin Integrator):** We synthesize absolute cellular conditions mimicking internal human physiology (**310 Kelvin**), aqueous viscosity resistance (**$1/ps$**), and the exact theoretical atomic step limit (**2 Femtoseconds**).
3. **Volumetric Survival (Monte Carlo Barostat):** We rigidly enforce a `1.0 Atm` pressure boundary. This mathematical sentry forcefully recalculates and restricts the spatial geometric box every 25 kinetic frames, preventing lethal density drops.
4. **The Trajectory Matrix (DCD Recorders):** A continuous hyper-speed macroscopic camera explicitly tracking Cartesian $(x,y,z)$ atomic shifts, seamlessly mapped into a `.dcd` video sequence.

---

## 🛠️ The Tech Stack Grid

<div align="center">

| Category | High-Performance Technologies |
| :--- | :--- |
| **Frontend Architecture** | ![](https://img.shields.io/badge/React_18-0EA5E9?style=flat-square&logo=react&logoColor=white) ![](https://img.shields.io/badge/Vite-F59E0B?style=flat-square&logo=vite&logoColor=white) ![](https://img.shields.io/badge/TailwindCSS-0EA5E9?style=flat-square&logo=tailwindcss&logoColor=white) ![](https://img.shields.io/badge/Framer_Motion-E10098?style=flat-square&logo=framer&logoColor=white) |
| **Serverless Backend** | ![](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white) ![](https://img.shields.io/badge/Modal_GPU_(A10G)-7C3AED?style=flat-square&logo=nvidia&logoColor=white) ![](https://img.shields.io/badge/Python_3.10-3776AB?style=flat-square&logo=python&logoColor=white) |
| **Computational Physics**| ![](https://img.shields.io/badge/OpenMM_8-0EA5E9?style=flat-square) ![](https://img.shields.io/badge/PDBFixer-F59E0B?style=flat-square) ![](https://img.shields.io/badge/MDTraj-7C3AED?style=flat-square) ![](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)|

</div>

---

## ⚔️ Engineering War Room (Case Studies)

Deploying intensive Python CUDA scripts over lightweight React boundaries exposed catastrophic limits requiring immediate logical interceptions:

### 1. The PBC Boundary Artifact (The "Escaping" Protein)
* **Problem:** Infinite explicitly solvated trajectories tracked using Periodic Boundary Conditions (PBC) fractured over 1M iterations, rendering the protein graphically torn outside the central water box.
* **Solution:** Reconfigured `minimization.py` by integrating `mdtraj`. The engine now executes `traj.image_molecules()` to meticulously redefine Cartesian bounds, wrapping the fragment perfectly back to the center *before* the `.dcd` returns to the client.

### 2. WebGL VRAM Crash (`setFrames` TypeError)
* **Problem:** Injecting multi-megabyte `DCD` payloads directly into the `$3Dmol.js` viewer instantly wiped out the React canvas, triggering memory context loss.
* **Solution:** Decoupled trajectory sequences inside `Viewer3D.jsx`. We isolated the molecular model (`const m = viewer.addModel()`) and safely injected the payload exclusively to the model instance via dynamic `try/catch` routines, securing 1-Million step capacities with zero frame drops.

### 3. AlphaFold Topology Corruption
* **Problem:** Raw AI predictions frequently supplied unrecognized `UNK` arrays and uncapped sequences, causing lethal OpenMM forcefield initialization crashes.
* **Solution:** Engineered a **Two-Pass Repair** execution inside `pdb_prep.py`. Pass 1 aggressively strips topological incompatibilities. Pass 2 dynamically reloads and repairs the matrix—appending specific missing geometric heavy atoms explicitly capped physically at **pH 7.4**.

---

## 👨‍🔬 Principal Architect

> [!IMPORTANT]
> **Hassan Ahmed Hassan Zaki Deraz**
> First-Year Undergraduate • Faculty of Agriculture, Alexandria University, Egypt.
> Accepted into **GCI World 2026** at the **Matsuo Laboratory, University of Tokyo**.

Operating completely solo during his early undergraduate tenure, Hassan designed this computational infrastructure to seamlessly converge rigorous traditional Agricultural Sciences with generative Machine Learning ecosystems. His architectural goal: destroying the barrier to High-Performance Physics (HPC) by democratizing supercomputing capabilities through accessible web interfaces.

<div align="center">
  
<a href="https://hassan-ahmed-portfolio.vercel.app"><img src="https://img.shields.io/badge/Portfolio-hassan--ahmed-0EA5E9?style=for-the-badge&logo=googlechrome&logoColor=0D1117&labelColor=0D1117" alt="Portfolio"/></a>&nbsp;&nbsp;
<a href="https://www.linkedin.com/in/hassan-ahmed2007"><img src="https://img.shields.io/badge/LinkedIn-Connect-0EA5E9?style=for-the-badge&logo=linkedin&logoColor=0EA5E9&labelColor=0D1117" alt="LinkedIn"/></a>&nbsp;&nbsp;
<a href="https://github.com/HassanAhmed2Ha"><img src="https://img.shields.io/badge/GitHub-HassanAhmed2Ha-0EA5E9?style=for-the-badge&logo=github&logoColor=white&labelColor=0D1117" alt="GitHub"/></a>

</div>

<br/>

<img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/aqua.png" width="100%" alt="divider"/>

<div align="center">
  <p><i>"We don't just study biology — we build the systems that compute it."</i></p>
  <p><b>BioPhys Nexus</b> • MIT License</p>
</div>
