# From Quantum Espresso to DeePMD-kit

<p align="justify">
I am sharing my personal experience of using DeePMD-kit with Quantum Espresso. As a beginner in atomistic modeling, I initially struggled with generating training data from Quantum Espresso outputs for use in DeePMD-kit. This tutorial is the result of those early challenges, and I am sharing it in the hope that it will help others who are just getting started. I am still learning, but I believe you may find this tutorial helpful if you are facing similar difficulties.

</p>

##Step 1: Quantum Espresso installation

I recommend the [Tutorial by Pranab Das](https://pranabdas.github.io/espresso/) for installation guidance and other essential information. It’s an excellent resource for anyone getting started with Quantum Espresso.

##Step 2: DeePMD-kit installation

First Download the DeePMD file from the github using following command:

```bash
wget https://github.com/deepmodeling/deepmd-kit/releases/download/v3.1.0/deepmd-kit-3.1.0-cpu-Linux-x86_64.sh
```
Make it executable:
```bash
chmod +x deepmd-kit-3.1.0-cpu-Linux-x86_64.sh
```
Run it:
```bash
./deepmd-kit-3.1.0-cpu-Linux-x86_64.sh
```
You have to click "Enter" mupltiple times to review the license agreement.

For location you will see somthing like this:

```bash
deepmd-kit will now be installed into this location:
/home/didar/deepmd-kit

- Press ENTER to confirm the location
- Press CTRL-C to abort the installation
- Or specify a different location below

```
After that it will ask for shell config:

```bash
conda config --set auto_activate_base false

You can undo this by running conda init --reverse $SHELL? [yes|no]
[no] >>> 
```

Type "Yes" If you want ready to use in any terminal.

If you type no (default) Conda won't auto-initialize. You’ll need to run this manually in every new terminal before using conda:

```bash
source /home/didar/deepmd-kit/etc/profile.d/conda.sh
```
To Test the installation
Run:
```bash
dp -h
```












