# Model Application


Once the DP model is trained and compressed, you can use it to perform molecular dynamics (MD) simulations in **LAMMPS**.


### Step 1: Navigate to LAMMPS Folder

Switch to the directory that contains your LAMMPS setup:

```bash
cd ../02.lmp
```
### Step 2: Link the DP Model

Create a symbolic link to the trained model file (graph-compress.pb) located in your training directory:
```bash
ln -s ../01.train/graph-compress.pb
```
After linking, you should see the following files:
```bash
ls
conf.lmp  graph-compress.pb  in.lammps
```

### Step 3: LAMMPS Input Script

Open in.lammps and note two key lines:
```bash
pair_style  deepmd graph-compress.pb
pair_coeff  * *
```
These lines tell LAMMPS to use the Deep Potential model (graph-compress.pb) to compute atomic interactions.

### Step 4: Run the Simulation

Execute LAMMPS as usual:
```bash
lmp -i in.lammps
```
After the run completes, you’ll find:
```bash
    log.lammps: Thermodynamic information during the simulation

    ch4.dump: Atomic trajectories

```
## Summary
By now, you have learned the basic workflow of using DeePMD-kit, which includes extracting training data from DFT calculations (such as those performed with Quantum ESPRESSO), converting that data into DeePMD-compatible format, training a deep potential model, freezing and compressing the trained model, running molecular dynamics simulations in LAMMPS using the DP model, and performing model inference through Python to analyze properties like energy from simulation trajectories.

For further reading, documentation, updates, and advanced features, please refer to the official GitHub repository:

- [DeePMD-kit GitHub](https://github.com/deepmodeling/deepmd-kit/)

