# QTrain
Repository for quantum training at LuxProvide

### Setup the environment

We need a python environment to install necessary dependencies. First, let's create a python environment. `micromamba` is a good, fast and lightweight replacement of conda, but one can use any method to create a virtual environment.

```bash
micromamba create -n qiskit python=3.12
```
This will install a virtual environment named `qiskit` with python version 3.12 and pip. Next, we activate the environment -

```bash
micromamba activate qiskit
```

Then we simply install the dependencies from the [requirements.txt](./requirements.txt) file.

```bash
pip install -r requirements.txt
```

> Note
> `qiskit-terra` seems to be incompatible with `qiskit>=1.0` as it creates importerror. See the details [here](https://docs.quantum.ibm.com/migration-guides/qiskit-1.0-installation#import-qiskit-raises-importerror).