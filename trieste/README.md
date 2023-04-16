# ICTP 2023 - Getting Started with TKET

Welcome to ICTP!

This page will provide a quick intro to TKET and help you set up a programming environment with all the necessary packages installed.

If you have any questions during the summer school please feel free to ask me (Callum) or one of the other Quantinuum team members (Kathrin or Yoshi).

We have also created a [public slack channel](https://tketusers.slack.com/join/shared_invite/zt-18qmsamj9-UqQFVdkRzxnXCcKtcarLRA#/shared-invite/email) for support and discussion.

## A Brief Introduction to TKET

TKET is a quantum computing toolkit and optimising compiler designed to extract the best performance from the Noisy Intermediate Scale Quantum (NISQ) computers of today. 

The core of TKET is a high performance C++ library which the user can access via the pytket python module. There are also several extension modules which allow the user to interface with a range of devices and popular quantum computing libraries (qiskit, cirq and more).

### Useful links

- The TKET source code is on [github](https://github.com/CQCL/tket).
- We have a [user manual](https://cqcl.github.io/pytket/manual/index.html). 
- See the API documentation for [pytket](https://cqcl.github.io/tket/pytket/api/index.html) and the [pytket-extensions](https://cqcl.github.io/pytket-extensions/api/index.html).
- The [notebook examples](https://github.com/CQCL/pytket/tree/main/examples) are also a useful resource.

## Setting up a python environment

In order to manage python installations with minimal hassle we recommend setting up a virtual environment. Virtual environments give you a place to install python packages and keep them separate from the other packages installed on your system.

```shell
python3 -m venv tket-env
```

Now you can activate your virtual environment as follows...

```shell
source tket-env/bin/activate
```

Now that we have activated our virtual environment we can install packages inside this tket-env environment using the pip package manager.

```shell
pip install matplotlib
```

If you are on Windows, replace the second command with tket-venv\Scripts\activate.bat. Note that if you are using PowerShell or another shell (such as fish or csh), the script to run in the second command may change, please refer to the [documentation](https://docs.python.org/3/library/venv.html) for details.

I have shown how to set up virtual environments using venv because this comes pre-installed with python. There are several other virtual environment managers you could use instead. Alternatively you could manage dependencies using a docker container or using poetry.

## Installing pytket and its Extensions

TKET is accessible though its python module, pytket. This can be installed using the pip package manager.

```shell
pip install pytket
```

Extensions can be installed by adding a hyphen to the installation command followed by the extension name.

```shell
pip install pytket-qiskit
```

Note that you will need python 3.8, 3.9 or 3.10 to use the latest version of pytket. To check which python version you have currently use `` python --verison``

For further help with installation see the [installation troubleshooting](https://cqcl.github.io/tket/pytket/api/install.html). You can also ask on [slack](https://tketusers.slack.com/ssb/redirect) or grab one of us in person.

### Packages for the ICTP Hackathon

The hackathon challenges will make use of the following python packages...

- pytket
- pytket-qiskit
- qermit

All of these can be installed with pip as shown above.

## Getting Started with TKET

Lets start by building a basic circuit. Those of you that have used qiskit before will see that pytket has a similar syntax.

Note one difference in how rotation gates work. In pytket rotation parameters are specified as the number of half turns. So a rotation of 0.5 in pytket is equivalent to 

```python
from pytket import Circuit # Import Circuit class
from pytket import OpType # Import OpType enum

circ = Circuit(3) # Create a Circuit with three qubits
circ.H(0) # Add a Hadamard to the 0th qubit
circ.CX(0,1) # CNOT with control qubit 0, target qubit 1
circ.Ry(0.5, 1) # Parameterised rotations - specified as number of half turns
circ.add_gate(OpType.CCX, [0,1,2]) # Less common gates can be added using OpType enum
circ.measure_all() # Measure all qubits in the standard basis
```

Now a minimal example using the AerBackend from pytket-qiskit...

```python
from pytket import Circuit
from pytket.extensions.qiskit import AerBackend # import noiseless Aer simulator

# Building a Bell circuit
bell_circ = Circuit(2)
bell_circ.H(0)
bell_circ.CX(0,1)

# Running a basic simulation
backend = AerBackend() #initialise AerBackend
handle = backend.process_circuit(bell_circ, n_shots=1000) # process_circuit for 1000 shots 
result = backend.get_result(handle) # Retrieve result from handle
counts_dict = result.get_counts()
print(counts_dict) # print a dictionary of results {basis states:counts}
```