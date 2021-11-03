# Introduction

The aim of this workshop is to get some experience of running solid-state (periodic) electronic structure calculations. You should adapt the notebooks, which Jarvist wrote to calculate for CsPbI3 cubic-perovskite material, to your own semiconductor from the active learning component of the lectures. As you achieve calculations in the lab, copy and paste the results into the collective document as a record of what you've done.

We are using ASE (the Atomic Simulation Environment) in a Jupyter notebook. ASE is a python package which allows computational manipulation and calculation of solid-state structures. It can use multiple back-ends for doing the actual calculations. 

In this lab we will be using the GPAW which is based in projector-augmented wave (PAW) density functional theory on a real-space grid. 

Useful documentation on ASE and GPAW:
https://wiki.fysik.dtu.dk/ase/ 
https://wiki.fysik.dtu.dk/gpaw/documentation/documentation.html 

**Important note: Due to the limitations of computer time in the lab session, we are using a very approximate method of electronic structure! As such your calculations are NOT CONVERGED. 
However, the same methods are used (with considerably larger computational demands, and calculation jobs that run over-night on super-computers) for doing publication quality work.**

## 01-ASE-GPAW-BandStructure.ipynb - Your first band structure calculation

Here is a Jupyter notebook using ASE and GPAW to do a DFT + Band Structure calculation on a Perovskite material. 
As everything is set up for the cubic perovskite space group (221), adapting the code to another cubic perovskite is fairly trivial. It is more work to adapt the codes to a different space group! 
As such, BaTiO3 is already present as an alternative if you want to initially try changing things just a little bit. 

- First work through the cells (from top to bottom), and try to understand what the Python ASE/GPAW code is doing, and what the codes are telling you. 
- Then start again from the top, but introducing a new material - such as the one you were compiling information for in the active learning component of the first lecture
- The aim for the end of the lab is to have calculated, for your material, the: 
 -  Band structure
 -  Band gap and nature of the excitation (indirect / direct)
 -  How do these values change depending on functional / k-point convergence ?
 -  How different is this band structure / band gap from the materials project data for the same material, and from experimental data that you can find?

## 02-ASE-GPAW-EffectiveMass.ipynb - Extracting an effective mass 'by hand'

This notebook builds on last year's tutorial where Jarvist worked through calculating an effective mass, by inspecting the energy eigenvalues, and building up (from very little!) the relationship between energy and momentum.
- In the lecture we could get an analytic form for the effective mass, from a tight-binding Bloch-wavefunction band structure, by using a small-angle approximation to cosine, and then taking the derivatives symbolically.
- In the workbook we assume a parabolic dispersion relationship (an effective mass electron), and then take a finite displacement relation to extract this curvature.
- This is essentially the same as the computation to get the band structure, except that we want to look just in the region immediately around an extremum point

You should:
 - Adapt the code to your semiconductor
 - Extract the dispersion relation (E(k)) near an extremum point,
 - Plot the dispersion relation. 
 - Fit a quadratic to it, and extract the effective mass
 - (This is entirely new, and not done on the workbook) Plot the effective mass dispersion relation, alongside the numeric band structure, and show how good the agreement is. This also works as a crosscheck that everything is correct.
Compare to literature data on the effective mass for your material.

## 03-ASE-GPAW-Phonons

This notebook calculates the phonon spectrum for a material

The main step uses a finite difference method, of perturbing the location of each atom in your structure in turn, to calculate the force-constants
Using these force-constants, one can then diagnosing the 'dynamic matrix' and so calculate the phonon dispersion relations (phonon band structure) and the phonon eigenvectors (the normal modes of the system)

These calculations take a lot of time! Initially you have some calculate force-constants, which the code is clever enough to reuse
Once you've played around for it a bit - run the commented out 'ph.clean()' command, which will delete these data

You should then:
 - Adapt the code to your material
 - Calculate the phonon dispersion relation and density of states
 - Add this to your workbook!

Documentation:
https://wiki.fysik.dtu.dk/ase/ase/phonons.html 

# Setting up Jupyter / ASE / GPAW, under Conda

## Install Conda

```shell
wget https://repo.anaconda.com/miniconda/Miniconda3-py38_4.10.3-Linux-x86_64.sh
bash Miniconda3-py38_4.10.3-Linux-x86_64.sh 
```

## Install PEM environment

Also includes Jupyter, GPAW, NWCHEM, ASE, etc.

```shell
conda env create --file env-conda-PEM.yml
conda activate PEM
```

## Configure Jupyter with SSL + etc.

Following https://jupyter-notebook.readthedocs.io/en/stable/public_server.html#running-a-public-notebook-server

```shell
jupyter notebook --generate-config
jupyter notebook password
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mykey.key -out mycert.pem
```

Nb: This SSL key is self-signed, so Chrome etc. complain quite nastily

```shell
. jupyter_external_access.sh
```

## 2020 Imperial Jupyter server installation instructions

Alternatively, you can make use of the college supercomputer CX1. 

1. Ensure you are connected to the Imperial VPN.
2. Go to https://jupyter.rcs.imperial.ac.uk/ 
3. Login with your Imperial credentials and spin up an instance. A single CPU for 8 hrs should suffice.
4. You should see a Jupyter Hub instance looking something like the below. This is running on a shard of the Imperial CX1 cluster. Due to some pretty amazing RCS magic you also have access to your Imperial files on the HPC.
5. Congratulations! You can now put 'cloud computing' on your CV.
6. Open File / New / Terminal from the browser.
7. This scary black box is a linux terminal, running on a login node of CX1. 
8. Copy and paste this magic into a terminal and cross your fingers. (On my desktop workstation connected via wired ethernet at Imperial, this takes about 20 seconds. On CX1 it takes a few minutes. On a laptop with slow Wifi it could take as long as 20 minutes.) 
```shell
module load anaconda3/personal
conda create -y -n PEM python=3.8 ipykernel
source activate PEM
conda config --add channels conda-forge
conda install -y xtb-python ase gpaw nglview ipywidgets
python -m ipykernel install --user --name python3_PEM --display-name "Python 3 PEM"
```
9. Was it successful? If so, congratulations! Add 'Conda package manager in a cloud environment' to your CV.
10. From the Conda hub, Open File / New / Notebook. 
11. Select your brand new Kernel from the drop-down menu (called 'Python 3 PEM'):



Try some of the code like the below ase...
Hopefully it works!
