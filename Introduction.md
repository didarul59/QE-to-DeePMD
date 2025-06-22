# From Quantum ESPRESSO to DeePMD-kit: A Workflow for Atomistic Modeling
<img src="workflow.png" align="right" width="300" style="margin-left: 20px; margin-bottom: 20px;">

Modern atomistic simulations often bridge first-principles accuracy with machine learning speed. This tutorial provides a complete workflow starting from Density Functional Theory (DFT) calculations using Quantum ESPRESSO, through training a deep neural network interatomic potential using DeePMD-kit, and finally to running large-scale Molecular Dynamics (MD) simulations with LAMMPS using the trained model.

We specifically focus on the example of CH₄, demonstrating:

- How to run DFT-based molecular dynamics (MD) and variable-cell MD (vc-MD) to sample high-quality atomic configurations.
- How to extract structural, energetic, force, and stress data from Quantum ESPRESSO outputs.
- How to convert that data into a format suitable for DeePMD-kit and train a robust potential.
- How to validate and apply the trained model in LAMMPS for large-scale simulations.

All steps are technical and practical, with minimal theory.

---

## Prerequisites

- A Linux system with Quantum Espresso Installed init
- Python environment (recommended)
- DeePMD-kit installed
- [QE-to-DeePMD converter script]([https://github.com/deepmodeling/deepmd-kit/tree/master/tools/convert/qe](https://github.com/didarul59/QE-relax_to_DeePMD.git)) (or your own script)

---
