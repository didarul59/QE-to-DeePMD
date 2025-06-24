# DFT Calculations
## Step 1: Run MD in Quantum Espresso
- Perform the Molecular Dynamics (MD) with enabling stress and force. It will produce relaxed atomic position at the specific given temperature.
```bash
 &control
    calculation = 'md'
    prefix='silicon',
    etot_conv_thr =   1.2000000000d-04
    forc_conv_thr =   1.0000000000d-04
    pseudo_dir = '/home/didar/pseudopot/',
    outdir='./'
    tstress = .true.,
    tprnfor = .true.,
    nstep = 500 ,
    dt = 20,              
    verbosity = 'high'
 /
 &system
    ibrav = 0
    nat   = 5
    ntyp  = 2
    ecutwfc=30,
    ecutrho = 240,
    nosym=.true.

 /
 &electrons
    conv_thr =  1.0d-7
 /
&IONS
  ion_temperature = 'rescale-T',
  tempw = 300,
  nraise = 1,       ! Rescale every step
  delta_t = 3.0
/

ATOMIC_SPECIES
C      15.99940  C.pbe-n-kjpaw_psl.1.0.0.UPF
H       1.00794  H.pbe-kjpaw_psl.1.0.0.UPF


CELL_PARAMETERS {angstrom}
 11.461500   0.000000   0.000000
  0.000000  11.613000   0.000000
  0.000000   0.000000  11.581300


ATOMIC_POSITIONS {angstrom}
C       5.730770   5.806480   5.790650
H       6.284870   6.606080   6.287150
H       6.414070   4.993080   5.537050
H       4.952570   5.432980   6.459850
H       5.271470   6.193880   4.878550
```
Run pw.x in self consistent mode. Remember you have to put MD over scf in the calculation.
```bash
pw.x < pw.md.in > pw.md.out
# For parallel execution
mpirun -np 4 pw.x -inp < pw.md.in > pw.md.out
```
Collect the Atomic Position from the last steps of the MD simulation. It will be the atomic positon for the VC-md calculation.
```bash
ATOMIC_POSITIONS (angstrom)
C                5.7334773081        5.8024699213        5.7913607903
H                6.2545090243        6.5830450255        6.3902672848
H                6.4099327761        4.9586685623        5.4472273628
H                4.8877026768        5.4750308881        6.4399882797
H                5.3278614319        6.2729289691        4.8738344379
```

## Step 2: Run the VC-MD Calculation

Remember you have to put the atomic positon of MD calculation to the input of the VC-MD or it will face convergence or thermal instability issues.

```bash
 &control
    calculation = 'vc-md'
    prefix='silicon',
    etot_conv_thr =   1.2000000000d-04
    forc_conv_thr =   1.0000000000d-04
    pseudo_dir = '/home/didar/pseudopot/',
    outdir='./'
    tstress = .true.,
    tprnfor = .true.,
    nstep = 500 ,
    dt = 20,                 
    verbosity = 'high'
 /
 &system
    ibrav = 0
    nat   = 5
    ntyp  = 2
    ecutwfc=30,
    ecutrho = 240,
    nosym=.true.

 /
 &electrons
    conv_thr =  1.0d-7
 /
&IONS
  ion_temperature = 'rescale-T',
  tempw = 300,
  nraise = 1,       ! Rescale every step
  delta_t = 3.0
/
&CELL
  cell_dynamics = 'pr',
  press = 0.3,              ! Target external pressure (GPa)
  press_conv_thr = 0.1,     ! Convergence threshold for pressuren
  cell_dofree = 'all',      ! Relax volume and shape
/

ATOMIC_SPECIES
C      15.99940  C.pbe-n-kjpaw_psl.1.0.0.UPF
H       1.00794  H.pbe-kjpaw_psl.1.0.0.UPF


CELL_PARAMETERS {angstrom}
 11.461500   0.000000   0.000000
  0.000000  11.613000   0.000000
  0.000000   0.000000  11.581300

ATOMIC_POSITIONS (angstrom)
C                5.7334773081        5.8024699213        5.7913607903
H                6.2545090243        6.5830450255        6.3902672848
H                6.4099327761        4.9586685623        5.4472273628
H                4.8877026768        5.4750308881        6.4399882797
H                5.3278614319        6.2729289691        4.8738344379

```
Run pw.x in self consistent mode..
```bash
pw.x < pw.vcmd.in > opt.out
# For parallel execution
mpirun -np 4 pw.x -inp < pw.vcmd.in > opt.out
```
You have to name the vc-md output as opt.out if you want to use my data conversion code.
