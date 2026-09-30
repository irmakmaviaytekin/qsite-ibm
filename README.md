# Q-SITE Hacks 2026 — Quantum Simulation of Hadron Dynamics

**Top 8% out of 250 teams at Q-SITE Hacks 2026**

This repository contains our solution to the Q-SITE Hacks 2026 quantum computing challenge, focused on simulating **hadron dynamics in the Schwinger model** using Qiskit and IBM Quantum hardware.

The project explores how a hadron wavepacket propagates in the **1+1 dimensional Schwinger model**, a simplified model of quantum electrodynamics. The goal was to construct, verify, optimize, and execute a large-scale quantum simulation while accounting for the limitations and noise of current quantum hardware.

At full scale, the simulation uses a lattice of **34 spatial sites mapped to 68 qubits** and studies the evolution of a hadron wavepacket to \(t=8\).

## Project Overview

The project combines concepts from quantum physics, numerical simulation, and quantum computing.

Our workflow included:

- constructing the lattice Schwinger-model Hamiltonian
- mapping the physical system onto qubits
- preparing the vacuum using **SC-ADAPT-VQE**
- preparing a localized hadron wavepacket
- implementing second-order **Suzuki-Trotter time evolution**
- validating quantum circuits against classical simulations
- transpiling large circuits for IBM quantum hardware
- selecting hardware-aware qubit layouts
- applying noise-mitigation techniques
- comparing hardware results against Matrix Product State (MPS) reference simulations

The primary observable is the **vacuum-subtracted chiral condensate**, which allows us to track the propagation of the hadron wavepacket across the lattice.

## Physics

The challenge is based on the **Schwinger model**, quantum electrodynamics in one spatial dimension.

Using staggered fermions and a Jordan-Wigner transformation, the lattice system is mapped onto qubits. For \(L=34\) spatial sites, the resulting quantum simulation requires:

**68 qubits**

The initial vacuum is prepared using a shallow variational circuit based on **SC-ADAPT-VQE**, after which a localized excitation is introduced to create the hadron wavepacket.

Time evolution is approximated using a symmetric second-order Suzuki-Trotter decomposition:

\[
U_2(\Delta t)
=
e^{-i\frac{\Delta t}{2}H_{\mathrm{kin}}}
e^{-i\Delta t H_{\mathrm{el}}}
e^{-i\Delta t H_m}
e^{-i\frac{\Delta t}{2}H_{\mathrm{kin}}}.
\]

The resulting chiral-condensate profile provides a way to visualize the propagation of the excitation through the lattice.

## Verification Before Hardware

A major focus of the challenge was verifying the physics and numerical approximations before using limited QPU time.

We tested and analyzed:

- Hamiltonian construction and charge-sector consistency
- electric-layer circuit identities
- SC-ADAPT-VQE vacuum fidelity
- Trotterization error
- Richardson extrapolation
- Hamiltonian truncation effects
- MPS bond-dimension convergence

The 68-qubit simulation was compared against classical **Matrix Product State** reference calculations before hardware execution.

This "trust, but verify" approach helped separate errors caused by physical approximations from errors introduced by quantum hardware.

## Hardware-Aware Quantum Computing

Running a 68-qubit physics simulation requires considerably more than constructing the logical circuit.

The project included hardware-aware engineering such as:

- calibration-aware qubit-chain selection
- ISA circuit transpilation
- physical qubit layout optimization
- two-qubit gate accounting
- QPU usage estimation
- Pauli twirling
- dynamical decoupling
- readout-error mitigation experiments

The repository also contains the layouts, runtime options, flight plans, and execution metadata used for the hardware workflow.

## Error Mitigation

One of the most interesting parts of the project was implementing **Operator Decoherence Renormalization (ODR)**.

Instead of relying exclusively on built-in mitigation tools, we constructed calibration circuits with known ideal expectation values and used them to estimate how noise suppresses measured observables.

The workflow included:

1. running physics and calibration circuits with similar gate structures
2. estimating site-dependent decoherence factors
3. renormalizing measured expectation values
4. propagating statistical uncertainties
5. comparing mitigated results against classical MPS references

Additional mitigation strategies explored in the project include **Pauli twirling, dynamical decoupling, and readout mitigation** through Qiskit Runtime.

## Repository Structure

```text
qsite-ibm/
│
├── schwinger_hadron_participant.ipynb
│   Main challenge notebook and implementation
│
├── challenge_utils.py
│   Challenge utilities and supporting functions
│
├── build-and-run-your-first-quantum-program.ipynb
│   Introductory IBM Quantum / Qiskit notebook
│
├── requirements.txt
│   Python dependencies
│
├── images/
│   Circuit diagrams and simulation figures
│
├── reference_data/
│   Classical and hardware reference data
│
└── submission/
    ├── circuits_isa.qpy
    ├── flight_plan.json
    ├── layout.json
    ├── mitigation_options.json
    └── hardware results and execution metadata
```

## Technologies

- **Python**
- **Qiskit**
- **Qiskit IBM Runtime**
- **Qiskit Aer**
- **NumPy**
- **SciPy**
- **Pandas**
- **Matplotlib**
- **Jupyter**
- IBM Quantum hardware and simulators

## Installation

Clone the repository:

```bash
git clone https://github.com/irmakmaviaytekin/qsite-ibm.git
cd qsite-ibm
```

Create a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows:

```bash
.venv\Scripts\activate
```

Install the required packages:

```bash
pip install -r requirements.txt
```

Then launch Jupyter:

```bash
jupyter notebook
```

The primary project notebook is:

```text
schwinger_hadron_participant.ipynb
```

## Results

Our submission placed in the **top 8% among 250 teams** participating in Q-SITE Hacks 2026.

Beyond the final ranking, this project provided hands-on experience connecting theoretical physics with practical quantum computing: moving from a Hamiltonian and physical model to quantum circuits, classical verification, hardware-aware compilation, execution on noisy quantum hardware, and statistical error mitigation.

## References

This project builds on work in large-scale digital quantum simulation of the Schwinger model, including:

- *Quantum Simulations of Hadron Dynamics in the Schwinger Model using 112 Qubits* — arXiv:2401.08044
- *Scalable Circuits for Preparing Ground States on Digital Quantum Computers: The Schwinger Model Vacuum on 100 Qubits* — arXiv:2308.04481

Parts of the challenge's physics narrative, figures, helper utilities, and introductory circuit material were adapted by the challenge organizers from the IBM Quantum Developer Conference 2025 **Hadron Dynamics in the Schwinger Model** challenge. The Q-SITE edition introduced additional verification, hardware-engineering, mitigation, and experimental tasks.

## About

This project was completed for **Q-SITE Hacks 2026** as part of my ongoing exploration of quantum computing at the intersection of **computer science and physics**.

My broader interests include computational astrophysics, machine learning, scientific computing, and quantum computation.
