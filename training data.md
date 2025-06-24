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
You now have data and training folder for deepmd kit. Remember you have to modify the input.json for your deep neural network.

## Step 2: Training Process (Based on DeePMD-kit documentation)

The following describes the training process of the Deep Potential (DP) model using DeePMD-kit.

Go to the 01.train folder and start training by running:

```bash
dp train input.json
```
Upon starting, the terminal will display information about the data systems used:
```bash
DEEPMD INFO      ----------------------------------------------------------------------------------------------------
DEEPMD INFO      ---Summary of DataSystem: training     -------------------------------------------------------------
DEEPMD INFO      found 1 system(s):
DEEPMD INFO                              system        natoms        bch_sz        n_bch          prob        pbc
DEEPMD INFO           ../00.data/training_data/             5             7           22         1.000          T
DEEPMD INFO      -----------------------------------------------------------------------------------------------------
DEEPMD INFO      ---Summary of DataSystem: validation   --------------------------------------------------------------
DEEPMD INFO      found 1 system(s):
DEEPMD INFO                               system       natoms        bch_sz        n_bch          prob        pbc
DEEPMD INFO          ../00.data/validation_data/            5             7            5         1.000          T
```
It will also show the learning rate settings:
```bash
DEEPMD INFO      start training at lr 1.00e-03 (== 1.00e-03), decay_step 5000, decay_rate 0.950006, final lr will be 3.51e-08
```
If training proceeds correctly, the screen prints progress every 1000 batches, for example:
```bash
DEEPMD INFO    batch    1000 training time 7.61 s, testing time 0.01 s
DEEPMD INFO    batch    2000 training time 6.46 s, testing time 0.01 s
DEEPMD INFO    batch    3000 training time 6.50 s, testing time 0.01 s
...
DEEPMD INFO   batch   10000 training time 6.41 s, testing time 0.01 s
DEEPMD INFO    saved checkpoint model.ckpt
```
At the 10,000th batch, the model is saved as model.ckpt. Training and testing errors are also logged in lcurve.out.
Checking Training Results

After training, check the learning curve with:

cat lcurve.out

Example output:
```bash
#step       rmse_val       rmse_trn       rmse_e_val       rmse_e_trn       rmse_f_val       rmse_f_trn           lr
      0     1.34e+01       1.47e+01         7.05e-01         7.05e-01         4.22e-01         4.65e-01     1.00e-03
    ...
 999000     1.24e-01       1.12e-01         5.93e-04         8.15e-04         1.22e-01         1.10e-01      3.7e-08
1000000     1.31e-01       1.04e-01         3.52e-04         7.74e-04         1.29e-01         1.02e-01      3.5e-08
```
At the end of the training process, DeePMD-kit saves the model parameters in TensorFlow checkpoint files (e.g., `model.ckpt`). To use this model in simulations or inference tasks, you need to **freeze** it into a standalone `.pb` (protobuf) file.

To do this, simply run the following command:

```bash
dp freeze -o graph.pb
```

DeePMD-kit allows you to compress the frozen model to significantly improve performance and reduce memory usage—often by an order of magnitude. This is especially useful for running large-scale molecular dynamics simulations.

To compress the model, use the following command:
```bash
dp compress -i graph.pb -o graph-compress.pb
```
Upon execution, the terminal will display messages like:
```bash
DEEPMD INFO    stage 1: compress the model
DEEPMD INFO    built lr
DEEPMD INFO    built network
DEEPMD INFO    built training
DEEPMD INFO    initialize model from scratch
DEEPMD INFO    finished compressing

DEEPMD INFO    stage 2: freeze the model
DEEPMD INFO    Restoring parameters from model-compression/model.ckpt
DEEPMD INFO    840 ops in the final graph
```
This process outputs a final compressed model file named graph-compress.pb.

