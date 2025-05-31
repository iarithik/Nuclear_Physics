# Nuclear Physics Code

This repository contains Python notebooks covering four core topics in nuclear physics: Binding Energy, Drip Lines, Shell Model, and Spherical Harmonics. Each section includes code and plots illustrating the relevant formulas and concepts.

## 1. Binding Energy

Calculates the binding energy per nucleon using the semi-empirical mass formula (Weizsäcker formula). For a nucleus with mass number A = N + Z (where N = number of neutrons, Z = number of protons), the binding energy B(A, Z) is computed as:

* Volume term:
  E\_vol = a\_v · A
      a\_v = 15.5 MeV

* Surface term:
  E\_surf = a\_s · A^(2/3)
      a\_s = 16.8 MeV

* Coulomb term:
  E\_coul = a\_c · Z · (Z – 1) / A^(1/3)
      a\_c = 0.72 MeV

* Asymmetry term:
  E\_asym = a\_asym · (A – 2Z)^2 / A
      a\_asym = 23 MeV

* Pairing term:
  δ(A, Z) = a\_p / A^(3/4)
      a\_p = +34 MeV for even-even nuclei
      a\_p = 0 for odd-A nuclei
      a\_p = –34 MeV for odd-odd nuclei

The total binding energy:
B(A, Z) = E\_vol – E\_surf – E\_coul – E\_asym + δ(A, Z)

The code constructs lists for A (from 1 to some upper limit), computes each term, and then plots all five curves versus A. Finally, it plots the binding energy per nucleon:
BE/A = B(A, Z) / A

### What the code does

1. Defines constants a\_v, a\_s, a\_c, a\_asym.
2. Loops over A and chooses Z optimally (Z ≈ A/2).
3. Calculates E\_vol = a\_v · A.
4. Calculates E\_surf = a\_s · A^(2/3).
5. Calculates E\_coul = a\_c · Z(Z – 1)/A^(1/3).
6. Calculates E\_asym = a\_asym · (A – 2Z)^2 / A.
7. Determines a\_p based on parity, then E\_pair = a\_p / A^(3/4).
8. Computes B(A, Z) and divides by A to get BE/A.
9. Plots each individual term and BE/A versus A.

## 2. Drip Lines

Determines the proton and neutron drip lines and plots the β-stability curve in the (N, Z) plane, using a modified semi-empirical mass formula. Drip lines are defined by separation energies:

* Proton separation:
  S\_p(N, Z) = B(N, Z) – B(N, Z – 1)  → Proton drip if S\_p ≤ 0

* Neutron separation:
  S\_n(N, Z) = B(N, Z) – B(N – 1, Z)  → Neutron drip if S\_n ≤ 0

Coefficients:

* a\_v = 14.1 MeV
* a\_s = 13.0 MeV
* a\_c = 0.595 MeV
* a\_asym = 19 MeV
* a\_p as in Section 1

Steps:

1. Implement B(N, Z) with these coefficients.
2. For each N, Z up to A = 300, compute S\_p and mark proton drip where S\_p = 0.
3. Compute S\_n and mark neutron drip where S\_n = 0.
4. Compute β-stability:
   Z\_stab(A) ≈ A / \[2 + (a\_c/a\_asym) · A^(2/3)]
5. Plot:

   * Proton drip line (green)
   * Neutron drip line (brown)
   * β-stability curve (blue dots)
   * N = Z line (black)

## 3. Shell Model

Plots energy levels for a nuclear shell model with harmonic oscillator potential, an ℓ(ℓ + 1) term, and spin-orbit coupling. Formulas:

1. E\_ho = (N + 3/2) ℓ ω, N = 2n\_r + ℓ
2. E\_ll = D · ℓ(ℓ + 1), D = – 0.0225 ℓ ω
3. ℓ·s = \[j(j + 1) – ℓ(ℓ + 1) – 3/4]/2, C = – 0.1 ℓ ω
4. E\_{ls} = C · ℓ·s

Total:
E\_total = E\_ho + E\_ll + E\_{ls}

Code:

* Magic numbers: \[2, 8, 20, 50, 82, 126]
* ℓ ω = 1 unit
* Loops over N = 0..6, computes energies for all (ℓ, j) combos
* Labels each level (e.g., 1s1/2, 1p3/2)
* Shows energy splitting, indicates magic numbers

## 4. Spherical Harmonics

Generates and plots Y\_l^m(θ, φ) using SciPy. Formula:

Y\_l^m(θ, φ) = sqrt\[(2l + 1)/(4π) · (l – m)!/(l + m)!] · P\_l^m(cos θ) · exp(i m φ)

Steps:

1. Meshgrid: θ in \[0, π], φ in \[0, 2π]
2. Compute Y\_l^m(θ, φ) with `sph_harm(m, l, φ, θ)`
3. Surface radius R = |Y\_l^m| or Re\[Y\_l^m]
4. Plot 3D surface: x = R sin θ cos φ, y = R sin θ sin φ, z = R cos θ
5. Arrange plots: (2l + 1) per row, l = 0..l\_max, shared colorbar

---

### How to run each section

1. Clone this repo.
2. Install: `pip install numpy matplotlib scipy`
3. Open `Nuclear_Physics_Code.ipynb` in Jupyter.
4. Run cells top to bottom. Each section begins with a header like `# Binding Energy`, `# Drip Lines`, etc.

---

### Summary of Key Formulas

1. **Binding Energy**
   E\_vol = a\_v A, E\_surf = a\_s A^(2/3), E\_coul = a\_c Z(Z – 1)/A^(1/3)
   E\_asym = a\_asym (A – 2Z)^2/A, δ(A, Z) = ±34/A^(3/4) (even-even/odd-odd)
   B(A, Z) = E\_vol – E\_surf – E\_coul – E\_asym + δ

2. **Drip Lines**
   S\_p = B(N, Z) – B(N, Z – 1), S\_n = B(N, Z) – B(N – 1, Z)
   Z\_stab ≈ A / \[2 + (a\_c/a\_asym) · A^(2/3)]

3. **Shell Model**
   E\_ho = (N + 3/2)ℓω, E\_ll = Dℓ(ℓ + 1), E\_{ls} = Cℓ·s
   E\_total = E\_ho + E\_ll + E\_{ls}

4. **Spherical Harmonics**
   Y\_l^m = sqrt\[(2l + 1)/(4π) (l – m)!/(l + m)!] · P\_l^m(cos θ) · e^{i m φ}

Use this README to understand the structure and physics implemented in each notebook section.
