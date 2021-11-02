# Setting up Jupyter / ASE / GPAW, under Conda

## Install Conda

```shell
wget https://repo.anaconda.com/miniconda/Miniconda3-py38_4.10.3-Linux-x86_64.sh
bash Miniconda3-py38_4.10.3-Linux-x86_64.sh 
```

# Install PEM environment

Also includes Jupyter, GPAW, NWCHEM, ASE, etc.

```shell
conda env create --file env-conda-PEM.yml
conda activate PEM
```

# Configure Jupyter with SSL + etc.

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
