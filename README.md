# Quantum-computer-programming
Research and development of coding in the field of emerging quantum computers.


What are the Q# programming language and Quantum Development Kit (QDK)?
----
Q# is Microsoft’s open-source programming language for developing and running quantum algorithms. It’s part of the Quantum Development Kit (QDK), which includes Q# libraries, quantum simulators, extensions for other programming environments, and API documentation. In addition to the Standard Q# library, the QDK includes Chemistry, Machine Learning, and Numeric libraries.

As a programming language, Q# draws familiar elements from Python, C#, and F# and supports a basic procedural model for writing programs with loops, if/then statements, and common data types. It also introduces new quantum-specific data structures and operations.



How does Q# work?
---
A Q# program can compile into a standalone application or be called by a host program that is written either in Python or a .NET language.

When you compile and run the program, it creates an instance of the quantum simulator and passes the Q# code to it. The simulator uses the Q# code to create qubits (simulations of quantum particles) and apply transformations to modify their state. The results of the quantum operations in the simulator are then returned to the program.

Isolating the Q# code in the simulator ensures that the algorithms follow the laws of quantum physics and can run correctly on quantum computers.

![qsharp-code-flow](https://user-images.githubusercontent.com/13979489/121571121-4a118a00-ca2b-11eb-96e0-a0712902ff85.png)


Qubits as opaque references
---
In Q# qubits are modeled as opaque data types that represent a reference to a specific two-state quantum system, whether physical or logical (error-corrected), on which quantum operations such as Hadamard or Pauli X may be performed. This is an operational view of qubits: qubits are defined by what you can do to them.

The representation used in Q# has the interesting implication that all of the actual quantum computing is done by side effect. There is no way to directly interact with the quantum state of the computer; it has no software representation at all. Instead, one performs operations on qubit entities that have the side effect of modifying the quantum state. Effectively, the quantum state of the computer is an opaque global variable that is inaccessible except through a small set of accessor primitives (measurements) — and even these accessors have side effects on the quantum state, and so are really “mutators with results” rather than true accessors.

Therefore, Q# has no ability to introspect into the state of a qubit or other properties of quantum mechanics directly, which guarantees that a Q# program can be physically executed on any quantum computer. Instead, a Q# program can call operations such as Measure to extract classical information from a qubit.

Programing quantum operations
---
Once allocated, a qubit can be passed to operations and functions, also referred to as callables. In some sense, this is all that a Q# program can do with a qubit. Any direct actions on state of a qubit are all defined by intrinsic callables such as X and H - that is, callables whose implementations are not defined within Q# but are instead defined by the target machine. What these operations actually do is only made concrete by the target machine you choose to run the particular Q# program. For more information, see Q# program implementation.

For example
---
if running the program on the full-state simulator, the simulator performs the corresponding mathematical operations to the simulated quantum system. But looking toward the future, when the target machine is a real quantum computer, calling such operations in Q# will direct the quantum computer to perform the corresponding real operations on the real quantum hardware. For example, in a trapped-ion quantum computer the quantum operations are realized by precisely timed laser pulses.

A Q# program
--- 
recombines these operations as defined by a target machine to create new, higher-level operations to express quantum computation. In this way, Q# makes it easy to express the logic underlying quantum and hybrid quantum–classical algorithms, while also being general with respect to the structure of a target machine or simulator.

A simple example is the following program, which allocates one qubit in the 
|0⟩
 state, then applies a Hadamard operation H to it and measures the result in the PauliZ basis.





# Optimization Solver: A Qiskit Function by Q-CTRL Fire Opal

### Overview

With the Fire Opal Optimization Solver, you can solve utility-scale optimization problems on quantum hardware without requiring quantum expertise. Simply input the high-level problem definition, and the Solver takes care of the rest. The entire workflow is noise-aware and leverages Fire Opal's Performance Management under the hood. The Solver consistently delivers accurate solutions to classically challenging problems, even at full-device scale on the largest IBM® QPUs.

The Solver is flexible and can be used to solve combinatorial optimization problems defined as objective functions or arbitrary graphs. Problems do not have to be mapped to device topology. Both unconstrained and constrained problems are solvable, given that constraints can be formulated as penalty terms. The examples included in this guide demonstrate how to solve an unconstrained and a constrained utility-scale optimization problem using different Solver input types. The first example involves a Max-Cut problem defined on a 156-node, 3-Regular graph, while the second example tackles a 50-node Minimum Vertex Cover problem defined by a cost function.

### Function description

The Solver fully optimizes and automates the entire algorithm, from error suppression at the hardware level to efficient problem mapping and closed-loop classical optimization. Behind the scenes, the Solver's pipeline reduces errors at every stage, enabling the enhanced performance required to meaningfully scale. The underlying workflow is inspired by the Quantum Approximate Optimization Algorithm (QAOA), which is a hybrid quantum-classical algorithm. For a detailed summary of the full Optimization Solver workflow, refer to the published manuscript.

![image](https://github.com/user-attachments/assets/1da79252-6e3a-4b1c-9b02-e63cdafc75d8)


### To solve a generic problem with the Optimization Solver:

- Define your problem as an objective function or a graph.
- Connect to the function through the Qiskit Functions Catalog.
- Run the problem with the Solver and retrieve results.


### Get started

```qs
from qiskit_ibm_catalog import QiskitFunctionsCatalog
 
# If you have not previously saved your credentials, follow instructions at
# https://docs.quantum.ibm.com/guides/setup-channel#iqp
# to authenticate with your API token.
catalog = QiskitFunctionsCatalog()
 
# Access Function
solver = catalog.load("q-ctrl/optimization-solver")
```

### Example: Unconstrained optimization

Run the maximum cut (Max-Cut) problem. The following example demonstrates the Solver's capabilities on a 156-node, 3-regular unweighted graph Max-Cut problem, but you can also solve weighted graph problems.

In addition to qiskit-ibm-catalog, you will also use the following packages to run this example: networkx and numpy. You can install these packages by uncommenting the following cell if you are running this example in a notebook using the IPython kernel.

```
# %pip install networkx numpy
```

#### Define the problem
```qs
import networkx as nx
import numpy as np
 
# Generate a random graph with 156 nodes
maxcut_graph = nx.random_regular_graph(d=3, n=156, seed=8)
```
```qs
# Optionally, visualize the graph
nx.draw(maxcut_graph, nx.kamada_kawai_layout(maxcut_graph), node_size=100)
```

![image](https://github.com/user-attachments/assets/53c7383a-bbbe-433c-a57d-0b9e6c4ef5c2)

The Solver accepts a string as the problem definition input.

```qs
# Convert graph to string
problem_as_str = nx.readwrite.json_graph.adjacency_data(maxcut_graph)
```

#### Run the problem

```qs
# Solve the problem
maxcut_job = solver.run(
    problem=problem_as_str,
    problem_type="maxcut",
    instance=instance,  # E.g. "ibm-q/open/main"
    backend_name=backend_name,  # E.g. "ibm_kyiv"
)
```
```qs
Check your Qiskit Function workload's status or return results as follows:
```
```qs
# Get job status
print(maxcut_job.status())
```


