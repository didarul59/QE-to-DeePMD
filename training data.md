# Data Conversion: From Quantum ESPRESSO to DeePMD-kit
<p align="justify">
For DeePMD training, we need force data, atomic position, cell parameter data from the DFT calculations. These properties are essential for accurately capturing the potential energy surface. While you can manually extract this information from Quantum ESPRESSO output files. I have provided a Python-based script that parses the required data directly from QE outputs and converts them into the format needed for DeePMD-kit training.
</p>

## Step 1:
First, let's grab the ready-to-use data conversion toolkit. Open your terminal and run:
```bash
wget https://github.com/didarul59/QE-relax_to_DeePMD.git
```
This package includes everything you need — no manual edits required!

Once the download is complete, simply copy your opt.out file (the Quantum ESPRESSO output) into the downloaded folder.

The Qe_to_deepmd folder should have these files:

```bash
❯ ls
collect.py  flat.py  README.md  reshape.py  shape.py  train_valid.py
combine.py  opt.out  rename.py  run.sh      to.py
```
Now, make the script executable and run it:
```bash
chmod +x run.sh
./run.sh
```
That's it! The script will automatically extract the atomic positions, forces, and cell parameters and prepare them in a DeePMD-compatible format. No hassle, just results!


