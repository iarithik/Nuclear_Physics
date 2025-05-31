# Nuclear Physics Code

This repository contains Python notebooks covering four core topics in nuclear physics: Binding Energy, Drip Lines, Shell Model, and Spherical Harmonics. Each section includes code and plots illustrating the relevant formulas and concepts.

## 1. Binding Energy

Calculates the binding energy per nucleon using the semi-empirical mass formula (Weizsäcker formula). For a nucleus with mass number A = N + Z (where N = number of neutrons, Z = number of protons), the binding energy B(A, Z) is computed as:

- Volume term:  
  Eₙₒₗ = aᵥ · A  
  &nbsp;&nbsp;&nbsp;&nbsp;– aᵥ = 15.5 MeV

- Surface term:  
  Eₛᵤᵣf = aₛ · A^(2/3)  
  &nbsp;&nbsp;&nbsp;&nbsp;– aₛ = 16.8 MeV

- Coulomb term:  
  E_cₒᵤₗ = a꜀ · Z · (Z – 1) / A^(1/3)  
  &nbsp;&nbsp;&nbsp;&nbsp;– a꜀ = 0.72 MeV

- Asymmetry term:  
  Eₐₛʸₘ = aₐₛᵧₘ · (A – 2Z)² / A  
  &nbsp;&nbsp;&nbsp;&nbsp;– aₐₛᵧₘ = 23 MeV

- Pairing term:  
  δ(A, Z) = a_p / A^(3/4)  
  &nbsp;&nbsp;&nbsp;&nbsp;• a_p = +34 MeV for even-even nuclei (both N and Z even)  
  &nbsp;&nbsp;&nbsp;&nbsp;• a_p = 0 for odd-A nuclei (one of N or Z is odd)  
  &nbsp;&nbsp;&nbsp;&nbsp;• a_p = –34 MeV for odd-odd nuclei (both N and Z odd)

The total binding energy is then:  
B(A, Z) = Eₙₒₗ – Eₛᵤᵣf – E_cₒᵤₗ – Eₐₛʸₘ + δ(A, Z)

The code constructs lists for A (from 1 to some upper limit), computes each term (Eₙₒₗ, Eₛᵤᵣf, E_cₒᵤₗ, Eₐₛʸₘ, Eₚₐᵢᵣ), and then plots all five curves versus A. Finally it plots the binding energy per nucleon:  
BE/A = B(A, Z) / A

### What the code does

1. Defines constants aᵥ, aₛ, a꜀, aₐₛᵧₘ.  
2. Loops over A and chooses Z optimally (for highest B, Z ≈ A/2 for β-stable nuclei).  
3. Calculates Eₙₒₗ = aᵥ · A.  
4. Calculates Eₛᵤᵣf = aₛ · A^(2/3).  
5. Calculates E_cₒᵤₗ = a꜀ · Z · (Z – 1) / A^(1/3).  
6. Calculates Eₐₛʸₘ = aₐₛᵧₘ · (A – 2Z)² / A.  
7. Determines pairing constant a_p based on parity of N and Z, then Eₚₐᵢᵣ = a_p / A^(3/4).  
8. Computes B(A, Z) and divides by A to get BE/A.  
9. Plots each individual term and BE/A versus A.

## 2. Drip Lines

Determines the proton and neutron drip lines and plots the β-stability curve in the (N, Z) plane, using another version of the semi-empirical mass formula. The drip lines are defined by the separation energies:

- Proton separation energy:  
  S_p(N, Z) = B(N, Z) – B(N, Z – 1)  
  Proton drip whenever S_p(N, Z) ≤ 0

- Neutron separation energy:  
  S_n(N, Z) = B(N, Z) – B(N – 1, Z)  
  Neutron drip whenever S_n(N, Z) ≤ 0

For (N, Z) pairs where the separation energy is zero or negative, proton or neutron emission becomes energetically favorable. The code uses the following coefficients:

- aᵥ = 14.1 MeV  
- aₛ = 13.0 MeV  
- a꜀ = 0.595 MeV  
- aₐₛᵧₘ = 19 MeV  
- a_p pairing constant chosen as in Section 1

The steps are:

1. Implement a function B(N, Z) identical to the binding‐energy formula in Section 1, using the coefficients above.  
2. For each N and Z in reasonable ranges (e.g. A up to 300), compute S_p(N, Z) and mark points where S_p = 0 as the proton drip.  
3. Similarly, compute S_n(N, Z) and mark points where S_n = 0 as the neutron drip.  
4. Compute the β-stability line:  
   Z_stab(A) ≈ A / [2 + (a꜀/aₐₛᵧₘ) · A^(2/3)]  
   which comes from minimizing B(A, Z) with respect to Z.  
5. Plot:
   - Proton drip line (green) in (N, Z) plane  
   - Neutron drip line (brown)  
   - β-stability curve (blue dots)  
   - N = Z line (black)  

## 3. Shell Model

Plots single-particle energy levels for a simple nuclear shell model with a harmonic‐oscillator potential, a term proportional to l(l + 1), and a spin-orbit coupling term. The relevant formulas:

1. Harmonic‐oscillator energy (no spin–orbit or l(l + 1) term):  
   Eₕₒ = (N + 3/2) · ℏω  
   where N = 2nᵣ + ℓ, nᵣ = radial quantum number, ℓ = orbital angular momentum quantum number.

2. l(l + 1) correction:  
   E_ℓℓ = D · ℓ · (ℓ + 1)  
   with D = –0.0225 · ℏω

3. Spin–orbit coupling:  
   E_{ℓ·s} = C · ℓ · s  
   but ℓ·s can be written in terms of total angular momentum j:  
   ℓ·s = [j · (j + 1) – ℓ · (ℓ + 1) – s · (s + 1)] / 2  
   with s = 1/2 and C = –0.1 · ℏω

4. Total single-particle energy:  
   E_total = (N + 3/2) ℏω + D · ℓ · (ℓ + 1) + C · ℓ·s

The code:

- Defines magic numbers: [2, 8, 20, 50, 82, 126].  
- Sets ℏω = 1 (in arbitrary energy units for plotting).  
- Assigns D = –0.0225 · ℏω, C = –0.1 · ℏω.  
- Loops over principal‐quantum‐shell index N = 0..6, and for each possible (ℓ, j) combination, computes Eₕₒ, E_ℓℓ, E_{ℓ·s}, and sums to get E_total.  
- Places a vertical stack of energy levels for each N, showing how levels split due to ℓ(ℓ + 1) and spin–orbit.  
- Labels each level with notation nᵣ(ℓ)_j (e.g. 1s₁/₂, 1p₃/₂, etc.) and indicates where magic numbers occur.

## 4. Spherical Harmonics

Generates and plots spherical harmonics Yₗᵐ(θ, φ) on the unit sphere for ℓ = 0..ℓₘₐₓ and m = –ℓ..+ℓ. Uses SciPy’s special functions. Main formula:

- Spherical harmonic (complex form):  
  Yₗᵐ(θ, φ) = √[ (2ℓ + 1)/(4π) · (ℓ – m)!/(ℓ + m)! ] · Pₗᵐ(cos θ) · e^{i m φ}

  where Pₗᵐ are the associated Legendre polynomials.

The code does the following:

1. Creates a 2D meshgrid of angles:  
   θ ∈ [0, π], φ ∈ [0, 2π].  
2. For each (ℓ, m) pair up to ℓₘₐₓ, computes Yₗᵐ(θ, φ) via SciPy’s `sph_harm(m, ℓ, φ, θ)`.  
3. Converts Yₗᵐ to a surface radius R = |Yₗᵐ| (or sometimes R = Re[Yₗᵐ]) and colors by phase or magnitude.  
4. Plots a 3D surface in spherical coordinates (x = R sin θ cos φ, y = R sin θ sin φ, z = R cos θ) for each (ℓ, m).  
5. Arranges all (2ℓ + 1) plots for a given ℓ in a row, ℓ from 0 to ℓₘₐₓ in successive rows, with a common color bar indicating amplitude.

---

### How to run each section

1. Clone this repository.  
2. Make sure you have **Python 3** with `numpy`, `matplotlib`, `scipy`, and `fractions` installed (e.g. via `pip install numpy matplotlib scipy`).  
3. Open the Jupyter notebook (e.g. `Nuclear_Physics_Code.ipynb`) and run the cells for each section in order.

Each section’s code cell begins with a header comment (`# Binding Energy`, `# Drip lines`, etc.) that clearly separates the four topics. The plots will appear inline, illustrating the quantitative behavior of each formula.

---

### Summary of Key Formulas

1. **Semi‐Empirical Mass Formula (Binding Energy per nucleon)**  
   Eₙₒₗ = aᵥ · A  
   Eₛᵤᵣf = aₛ · A^(2/3)  
   E_cₒᵤₗ = a꜀ · Z(Z – 1) / A^(1/3)  
   Eₐₛʸₘ = aₐₛᵧₘ · (A – 2Z)² / A  
   δ(A, Z) = { +34/A^(3/4) for even–even, 0 for odd–A, –34/A^(3/4) for odd–odd }  
   B(A, Z) = Eₙₒₗ – Eₛᵤᵣf – E_cₒᵤₗ – Eₐₛʸₘ + δ(A, Z)

2. **Drip Line Conditions**  
   S_p(N, Z) = B(N, Z) – B(N, Z – 1) ≤ 0 → Proton drip  
   S_n(N, Z) = B(N, Z) – B(N – 1, Z) ≤ 0 → Neutron drip  
   Z_stab(A) ≈ A / [2 + (a꜀/aₐₛᵧₘ) · A^(2/3)]

3. **Shell Model Energy Levels**  
   Eₕₒ = (N + 3/2) · ℏω  
   E_ℓℓ = D · ℓ(ℓ + 1), D = –0.0225 ℏω  
   ℓ·s = [ j(j + 1) – ℓ(ℓ + 1) – 3/4 ] / 2, C = –0.1 ℏω  
   E_{ℓ·s} = C · ℓ·s  
   E_total = Eₕₒ + E_ℓℓ + E_{ℓ·s}

4. **Spherical Harmonics**  
   Yₗᵐ(θ, φ) = √[ (2ℓ + 1)/(4π) · (ℓ – m)!/(ℓ + m)! ] · Pₗᵐ(cos θ) · e^{i m φ}

Use this README as a guide to understand the code structure and the nuclear‐physics formulas implemented in each section.
