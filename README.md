# Quantum Confinement in a 1D Infinite Potential Well

**Course:** 24BSE2113-D — Partial Differential Equations, Transforms & Optimization Techniques
**Department:** Electronics and Communication Engineering, Saintgits College of Engineering (Autonomous)
**Group 13:** Chriss Mathew Rajan (23) · Devika S Liju (25) · Renjitha Babu (55) · Sethuparvathy K J (58) · Sharon Thomas (61)

## Overview

This project derives and numerically verifies the quantized energy states of an electron confined to a 1D infinite potential well ("particle in a box"), starting from the 1D Time-Dependent Schrödinger Equation. It has two parts:

| File | Description |
|---|---|
| `LaTeX.tex` | Phase 1 — analytical derivation of the theory, from the governing PDE to the final Fourier series solution |
| `CODE.ipynb` | Phase 2 — Python notebook that computes, visualizes, and numerically verifies the results from Phase 1 |

## Phase 1 — Theoretical Derivation (`LaTeX.tex`)

Solves the equation `iℏ ∂Ψ/∂t = -(ℏ²/2m) ∂²Ψ/∂x²` for a particle trapped in a well of length `L` using **separation of variables**:

1. **Setup** — defines the infinite square well potential and the Dirichlet boundary conditions Ψ(0,t) = Ψ(L,t) = 0.
2. **Separation of variables** — assumes Ψ(x,t) = ψ(x)φ(t), splitting the PDE into a temporal ODE and a spatial ODE linked by a separation constant `E` (the energy).
3. **Temporal solution** — solves for φ(t) = C·exp(-iEt/ℏ), a pure phase rotation (stationary state).
4. **Spatial solution & boundary conditions** — solves the harmonic-oscillator-type spatial ODE, applies the boundary conditions, and shows only discrete wavenumbers `kₙ = nπ/L` are allowed.
5. **Quantized energy levels** — derives `Eₙ = n²π²ℏ²/(2mL²)`, the discrete energy ladder.
6. **Normalization** — fixes the amplitude constant so total probability equals 1, giving `ψₙ(x) = √(2/L)·sin(nπx/L)`.
7. **Final Fourier series solution** — combines all modes via the superposition principle into the general solution:

   `Ψ(x,t) = Σₙ cₙ √(2/L) sin(nπx/L) exp(-i n²π²ℏ t / 2mL²)`

   with coefficients `cₙ` fixed by the initial wave function via a Fourier projection integral.

**Key takeaway:** confinement is what forces the electron's energy to become discrete rather than continuous, and the spacing between allowed energies scales as `1/L²` — the physical basis of quantum dots, quantum wells, and nanoscale transistor channels.

## Phase 2 — Computational Verification (`CODE.ipynb`)

Implements and visualizes the Phase 1 theory for an electron in a well of width `L = 1.0 nm`, in seven stages:

1. **Constants & core functions** — physical constants (ℏ, electron mass, eV) and the analytical expressions for `ψₙ(x)` and `Eₙ`.
2. **Quantized energy ladder** — computes and plots `E₁`–`E₈`. Ground state `E₁ ≈ 0.376 eV`; spacing grows as `n²`.
3. **Eigenfunctions & probability densities** — plots `ψₙ(x)` and `|ψₙ(x)|²` for the first few modes, showing `n − 1` nodes per mode.
4. **Numerical verification of orthonormality** — computes the overlap matrix `∫ψₙψₘ dx` via trapezoidal integration; confirms it is the identity matrix (1 on the diagonal, 0 off it).
5. **Superposition state & Fourier coefficients** — projects an initial Gaussian wave packet (centered at `x = L/4`) onto the eigenbasis to get coefficients `cₙ`; confirms `Σ|cₙ|² = 1` (Parseval's theorem) and reconstructs the packet from 40 modes.
6. **Time evolution** — propagates the superposition state `Ψ(x,t)` over 0–5 femtoseconds, showing wave packet spreading and interference caused by the `n²`-dependent phase rates.
7. **Confinement-length dependence** — sweeps well width `L` from 0.1–100 nm and confirms `E₁ ∝ 1/L²` (a straight line of slope −2 on a log-log plot).

### Requirements

- Python 3
- `numpy`
- `matplotlib`

### Running the notebook

```bash
pip install numpy matplotlib
jupyter notebook project_detailed.ipynb
```
Run all cells top to bottom — each code cell depends on functions/variables defined earlier (e.g., `psi_n`, `energy_level`, `c_n`, `n_list`).

## Key Results Summary

| Quantity | Value |
|---|---|
| Ground-state energy (L = 1 nm) | E₁ ≈ 0.3760 eV |
| Energy scaling | Eₙ ∝ n² |
| Confinement scaling | E₁ ∝ 1/L² (verified: 10× smaller L → 100× larger E₁) |
| Orthonormality check | Overlap matrix = identity (to 3 d.p.) |
| Fourier reconstruction | 40-mode sum reproduces Gaussian wave packet; Σ|cₙ|² = 1.00000 |

## Engineering Implications

- Discreteness of energy levels arises purely from geometric boundary conditions, not from any special material property.
- The `1/L²` scaling is a practical design lever: shrinking a confined structure's size predictably increases the spacing between allowed energy states — used to tune absorption/emission wavelengths in quantum dots and lasers.
- Because the eigenbasis is orthonormal, any initial electron state can be decomposed into these modes and its future evolution predicted exactly — a method that extends to realistic 2D/3D confinement geometries in semiconductor device design.

## References

1. D. J. Griffiths, *Introduction to Quantum Mechanics*, 2nd ed., Pearson Prentice Hall, 2005.
2. E. Kreyszig, *Advanced Engineering Mathematics*, 10th ed., John Wiley & Sons, 2011.
3. R. Shankar, *Principles of Quantum Mechanics*, 2nd ed., Springer, 1994.
