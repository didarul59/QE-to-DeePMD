# Converting Quantum ESPRESSO Data to DeePMD Data

This guide provides step-by-step instructions to convert Quantum ESPRESSO output files for use in DeePMD-kit training. All steps are technical; no theoretical background provided.

---

## Prerequisites

- Quantum ESPRESSO completed calculation (trajectory and energy/force files)
- Python environment (recommended)
- DeePMD-kit installed
- [QE-to-DeePMD converter script](https://github.com/deepmodeling/deepmd-kit/tree/master/tools/convert/qe) (or your own script)

---

## Step 1: Collect Required Files

- Obtain the following from your QE run:
  - `pwscf.out` (or equivalent output with energies and forces)
  - Trajectory files (e.g., `md.xyz`, `md.traj`, or similar)

## Step 2: Install DeePMD-kit & Tools

```bash
pip install deepmd-kit
# or follow official instructions for your platform
```

Clone the converter tools:
```bash
git clone https://github.com/deepmodeling/deepmd-kit.git
cd deepmd-kit/tools/convert/qe
```

## Step 3: Run the Conversion Script

- Prepare your QE output files in the script directory (or provide the path).
- Run the conversion tool (example):

```bash
python qe2lmp.py -i /path/to/pwscf.out -o deepmd_data
```

- This generates a `deepmd_data` folder with DeePMD-compatible data.

**Note:** Script names and options may vary based on the converter version. Always check the `README.md` in the converter tool directory.

## Step 4: Verify the Converted Data

- Check that the `deepmd_data` folder contains files like:
  - `type.raw`
  - `coord.raw`
  - `box.raw`
  - `energy.raw`
  - `force.raw`
- These files are ready for DeePMD-kit training.

## Troubleshooting

- If the script fails, check:
  - File paths and names
  - Python dependencies
  - Converter script compatibility with your QE version

For more details, refer to:
- [DeePMD-kit Documentation](https://deepmd.readthedocs.io/en/latest/)
- [QE-to-DeePMD Converter](https://github.com/deepmodeling/deepmd-kit/tree/master/tools/convert/qe)

---

*End of technical guide*