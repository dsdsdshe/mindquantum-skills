# Built-in Ansatz Catalog

MindQuantum provides ready-to-use ansatz circuits in `mindquantum.algorithm.nisq`. All ansätze inherit from `Ansatz` base class and produce a `Circuit` via `.circuit`.

## Hardware Efficient Ansatz (HEA)

General-purpose ansatz with alternating rotation and entangling layers.

```python
from mindquantum.algorithm.nisq import HardwareEfficientAnsatz
from mindquantum.core.gates import RY, RZ, CNOT

ansatz = HardwareEfficientAnsatz(
    n_qubits=4,
    single_rot_gate_seq=[RY, RZ],   # rotation gates per layer
    entangle_gate=CNOT,              # entangling gate
    depth=3                          # number of layers
)
circ = ansatz.circuit
```

**When to use:** Default choice when you have no prior knowledge about the problem structure. Good for QML and generic variational tasks.

## Strongly Entangling Ansatz

Rotation layers with entangling gates at varying distances.

```python
from mindquantum.algorithm.nisq import StronglyEntanglingAnsatz

ansatz = StronglyEntanglingAnsatz(n_qubits=4, depth=1)
circ = ansatz.circuit
```

**When to use:** QML classification tasks. Produces more expressible circuits than simple HEA.

## QAOA Ansatz

For combinatorial optimization (Max-Cut, SAT):

```python
from mindquantum.algorithm.nisq import QAOAAnsatz
from mindquantum.core.operators import QubitOperator

# Cost Hamiltonian from graph edges
ham = QubitOperator('Z0 Z1') + QubitOperator('Z1 Z2') + QubitOperator('Z0 Z2')

ansatz = QAOAAnsatz(ham, depth=4)
circ = ansatz.circuit   # includes initial H layer + p layers of cost/mixer
```

**When to use:** Max-Cut, graph coloring, and constraint satisfaction problems.

## Max-Cut Ansatz

Specialized QAOA variant for Max-Cut problems:

```python
from mindquantum.algorithm.nisq import MaxCutAnsatz

ansatz = MaxCutAnsatz(
    graph=[(0,1), (1,2), (2,0)],   # edge list
    depth=4
)
circ = ansatz.circuit
```

## UCCSD Ansatz (Quantum Chemistry)

Unitary Coupled-Cluster Singles and Doubles for molecular ground state:

```python
from mindquantum.algorithm.nisq import UCCSDansatz

ansatz = UCCSDansatz(
    n_qubits=4,
    n_electrons=2,
    occ_orb=None,       # occupied orbitals
    vir_orb=None,       # virtual orbitals
    trotter_step=1
)
circ = ansatz.circuit
```

**When to use:** Quantum chemistry VQE. Physically motivated but deep circuits.

Also available:
- `UCCSD0` — simplified UCCSD variant
- `QUCCSD` — qubit-adapted UCCSD
- `UCCansatz` — general UCC from FermionOperators

## Hardware Efficient for Chemistry

Shallower alternative to UCCSD:

```python
from mindquantum.algorithm.nisq import HardwareEfficientAnsatz

# Use chemistry-tailored initial state + HEA
from mindquantum.core.gates import X
init_state = Circuit()
init_state += X.on(0)   # |01⟩ HF reference for 2 electrons
init_state += X.on(1)

ansatz = HardwareEfficientAnsatz(4, [RY, RZ], CNOT, depth=2)
full = init_state + ansatz.circuit
```

## IQP Encoding

Instantaneous Quantum Polynomial encoding for data re-uploading:

```python
from mindquantum.algorithm.nisq import IQPEncoding

encoding = IQPEncoding(n_feature=4, n_qubits=4, num_repeats=2)
circ = encoding.circuit
```

**When to use:** Feature encoding in QML when you want higher-order correlations.

## Ansatz Selection Guide

| Problem | Recommended Ansatz | Rationale |
|---------|-------------------|-----------|
| Generic optimization | `HardwareEfficientAnsatz` | Low depth, flexible |
| QML classification | `StronglyEntanglingAnsatz` | Expressive, trainable |
| Max-Cut / QAOA | `QAOAAnsatz` or `MaxCutAnsatz` | Problem-specific structure |
| Molecular ground state | `UCCSDansatz` | Chemical accuracy |
| Large molecule (approximate) | HEA + HF initial state | Shallower, GPU-friendly |
| Data encoding | `IQPEncoding` | Higher-order feature map |

## Key Properties

All ansätze expose:

```python
ansatz.circuit              # The parameterized Circuit
ansatz.n_qubits             # Number of qubits
ansatz.params_name          # List of parameter names
```
