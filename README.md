# MindQuantum Skills

Agent skills for [MindQuantum](https://atomgit.com/mindspore/mindquantum), the high-performance quantum computing framework. Install once, then build quantum circuits, train variational algorithms, simulate noisy systems, and solve combinatorial optimization problems — all guided by AI.

## Installation

```bash
npx skills add dsdsdshe/mindquantum-skills
```

Or install a specific skill:

```bash
npx skills add dsdsdshe/mindquantum-skills --skill mindquantum
```

## Skills

| Skill | Description |
|-------|-------------|
| **[mindquantum](skills/mindquantum/)** | Core API — circuit construction, gates, operators, simulator selection, common patterns |
| **[mq-variational-training](skills/mq-variational-training/)** | Train VQE, QAOA, and QML models with gradient-based optimization and MindSpore MQLayer |
| **[mq-noisy-simulation](skills/mq-noisy-simulation/)** | Simulate noisy quantum circuits — noise channels, ChannelAdder, density matrix backends |
| **[mq-qaia-solver](skills/mq-qaia-solver/)** | Solve combinatorial optimization with quantum-inspired algorithms (SimCIM, SB) on CPU/GPU/NPU |
| **[mq-circuit-compiler](skills/mq-circuit-compiler/)** | Compile circuits for hardware — gate decomposition, SABRE qubit mapping, topology management |
| **[mq-quantum-chemistry](skills/mq-quantum-chemistry/)** | Quantum chemistry VQE — molecular Hamiltonians, fermion transforms, UCCSD ansatz, mqchem |

## Which Skill Do I Need?

| I want to... | Skill |
|--------------|-------|
| Build and simulate a quantum circuit | `mindquantum` |
| Train a VQE or QAOA algorithm | `mq-variational-training` |
| Build a quantum classifier (QML) | `mq-variational-training` |
| Simulate noise effects on my circuit | `mq-noisy-simulation` |
| Solve Max-Cut or Ising problems on GPU | `mq-qaia-solver` |
| Compile my circuit for real hardware | `mq-circuit-compiler` |
| Compute molecular ground state energies | `mq-quantum-chemistry` |

## Quick Example

After installing, just describe what you want:

> "Build a 4-qubit QAOA circuit for Max-Cut on a triangle graph, optimize with BFGS, and show the solution."

The agent will use the appropriate skills to generate correct, idiomatic MindQuantum code.

## Requirements

- [MindQuantum](https://atomgit.com/mindspore/mindquantum) (`pip install mindquantum`)
- [MindSpore](https://www.mindspore.cn/install) (optional, for `MQLayer` hybrid training)
- [OpenFermion](https://github.com/quantumlib/OpenFermion) + [OpenFermion-PySCF](https://github.com/quantumlib/OpenFermion-PySCF) (optional, for quantum chemistry)

## License

Apache 2.0. See [LICENSE](LICENSE).
