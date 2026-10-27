# Physics and numerical scope

## Starting point

The supplied Standard Model Lagrangian includes:

- `SU(3)` gluon field-strength term, `SU(2)` electroweak field-strength term and `U(1)` field-strength term.
- Fermion kinetic terms coupled through the full gauge-covariant derivative.
- Higgs doublet kinetic term and quartic symmetry-breaking potential.
- Yukawa interactions among quarks, leptons and Higgs.
- The strong CP `θ₃ G ⋅ G̃` term.

A faithful, dynamical simulation of all of these terms is considerably more involved than this educational browser application. The implemented reduction is a **single complex scalar plus a single classical Abelian gauge field**.

## Evolved system

We evolve

```text
L_toy = -1/4 F_munu F^munu
        + (D_mu Phi)* (D^mu Phi)
        - lambda (|Phi|^2 - v^2/2)^2,
D_mu = partial_mu - i g A_mu.
```

In temporal gauge `A_0 = 0`, the scalar equation takes the form

```text
second_time_derivative(Phi)
    = sum_i (D_i D_i Phi)
      - 2 lambda (|Phi|^2 - v^2/2) Phi.
```

The spatial gauge fields evolve under lattice magnetic forces and the scalar gauge-link current. Field evolution is **classical**, not a path-integral calculation. The vacuum scale and masses are shown in *dimensionless* toy units.

### Lattice discretization

- Spatial dimensions: two, with a `64 × 64` periodic square lattice.
- Spatial spacing: `Δx = 1`.
- Time step: `Δt = 0.045`.
- Integrator: velocity-Verlet (with optional phenomenological damping applied to velocities).
- Gauge transport along each lattice link uses `U_i(x) = exp(-i g A_i(x) Δx)`.
- Scalar gradients use gauge-covariant nearest-neighbor differences.
- Gauge magnetic flux is calculated from oriented plaquette differences.
- Gauss residual monitors a discrete divergence of electric field minus scalar charge density; the application reports its root-mean-square value, not an enforced exact projection.

The displayed scalar phase is gauge-dependent. Higgs magnitude, energy density and magnetic flux provide more directly gauge-invariant diagnostics within the chosen formulation.

### Illustrative mass scales

```text
Radial scalar:     m_h = sqrt(2 lambda) v
Abelian gauge:     m_A = g v
Yukawa readout:    m_f = y v / sqrt(2)
```

These are formula-based illustrative scales. **No dynamical fermion field is present**, and the simulation does not reproduce the physical W/Z/Higgs mass spectrum or running Standard Model couplings.

## Initial conditions

1. **Quench:** randomized small field near the symmetric configuration, allowing symmetry-breaking growth and domain formation.
2. **Vortex pair:** two opposite scalar phase windings as an initial field configuration; not an assertion that a stable exact vortex solution is obtained.
3. **Pulse:** radial perturbation above a uniform scalar vacuum.

## What is not simulated

| Term or phenomenon | Status |
| --- | --- |
| Classical U(1) gauge field | Dynamically evolved |
| Single complex symmetry-breaking scalar | Dynamically evolved |
| Full SU(2) electroweak Yang–Mills field | Not implemented |
| SU(3) QCD gluon dynamics | Not implemented |
| Standard Model Higgs doublet | Reduced to a single complex scalar |
| Quark/lepton fields, spin, anti-commuting Grassmann variables | Not implemented |
| Yukawa interaction/fermion backreaction | Readout only, not implemented |
| QCD theta term or non-Abelian topology | Not implemented |
| Particle production, scattering cross sections, quantization, renormalization | Not implemented |
| Physical units or empirical parameter calibration | Not implemented |

This demonstration cannot verify the supplied full Standard Model Lagrangian, validate a proposed extension, or establish an experimental prediction.

## Numerical testing suggestions

- Turn **damping to zero** for energy-conservation checks; avoid changing couplings mid-run for this test.
- Pause, select **Pulse**, record `HiggsGaugeLab.getState().energyPerSite`, advance a chosen fixed number of steps and compare the energy.
- Watch `gaussRms` over time; if it grows appreciably, flag it as an integrator or constraint issue.
- Repeat for vortex and quench initial conditions, noting that quench seeds are random.
- Vary lattice resolution and time step in a future numerical implementation before claiming convergence; the browser lab fixes both quantities.
- Validate the reduction against known analytic benchmarks before attempting quantitative physics.

The JavaScript implementation, not this note, is the authoritative description of the code's numerical procedure. See `index.html` for the force, step, sample and rendering functions.

## Future work

For an electroweak-classical next step, evolve the actual `SU(2) × U(1)` gauge fields and Higgs doublet on a lattice with gauge constraint checks and convergent discretization. Coupling quantum dynamical fermions and SU(3) QCD would be a separate, substantially larger project.
