# graphene-photonic-memory-routes

**A Bayesian Pre-Experimental Decision Framework for Graphene Integration Route Selection in Ferroelectric Photonic Memory**

Nils Haaland · Solbakken Research Initiative · v9.1 · May 2026

---

## What this is

This repository contains the paper, simulation framework, and prior parameter files for a Bayesian Monte Carlo decision tool that ranks ten candidate graphene integration routes for ferroelectric photonic memory against five simultaneous optical quality criteria.

**No experimental prototype combining graphene gating, ferroelectric polarization, and photonic waveguide operation has been demonstrated.** This framework exists to answer one question before fabrication begins: *which routes are worth attempting first, and in what order?*

The paper is a pre-experimental decision tool, not a results paper. The route rankings are falsifiable theoretical predictions. The four experiments in Section 7 are the falsification protocol.

---

## The problem

Photonic matrix-vector multiplication (MVM) offers multiply-accumulate energies approaching 0.1 fJ/MAC — three to four orders of magnitude below DRAM-limited digital accelerators. The binding constraint is weight storage: volatile phase weights require DRAM reload overhead that dwarfs compute energy at scale (~1.69 J per inference pass for a 70B-parameter network under standard memory hierarchy assumptions).

Graphene-gated ferroelectric photonic memory (Al:HfO₂ / HZO family) is a proposed non-volatile solution. The integration route determines whether graphene quality meets the five-parameter Goldilocks window required for optical uniformity across a 64×64 MVM tile.

---

## The Goldilocks window (64×64 MVM, 0.5 dB IL budget)

| Parameter | Threshold | Physical basis |
|---|---|---|
| Defect density I_D/I_G | < 0.1 | Carrier scattering, conductivity modulation depth |
| Domain size L_domain | > 10 µm | Grain boundary insertion loss variance |
| Carrier mobility µ | > 3,000 cm²/V·s | Drude intraband absorption |
| Carrier density n* | < 10¹¹ cm⁻² | Pauli blocking contrast, Fermi level offset |
| Monolayer coverage P(mono) | > 95% area | Optical modulation depth uniformity |

Thresholds scale with array size: a 16×16 array relaxes n* by 4×; a 128×128 array tightens it by 2×.

---

## Principal results (N = 200,000 Monte Carlo samples)

| Route | P(all 5) | Binding constraint |
|---|---|---|
| A–D: UNCD graphitization | ~0% | Domain size (substrate physics wall) |
| E: SCD(111) direct | ~0% | Mobility / carrier density |
| **F: SCD(111) Ni+H-term** | **21.8%** → **57.2% at ceiling** | Mobility |
| G: CVD/Pt → SiO₂ | 13.7% | Carrier density → mobility |
| H: CVD/Cu → SiO₂ | 8.2% | Carrier density → mobility |
| **I: CVD/Pt → hBN** | **38.6%** | Mobility |
| J: CVD/Pt → SCD(111) | ~21–36% (midpoint 28.8%)† | Carrier density → mobility |

†Route J figure is an interval on an order-of-magnitude prior with no direct experimental measurement. Present as 21–36%, not 28.8%.

**Route F / Route I crossover:** 3,500–4,500 cm²/V·s mobility mean. Below this, Route I is the rational choice. Above it, Route F is superior on every dimension simultaneously.

---

## The four experiments (falsification protocol)

Ordered by decision value — each produces a specific number that updates a prior and resolves a design choice.

**Experiment 1 — Route F mobility mapping**
Measure post-H-plasma Hall mobility on SCD(111) Ni-assisted graphene (Ni 0.5–5 nm, RTA 850–1050°C). Determines whether the crossover threshold is reachable at current toolsets or requires a dedicated mobility improvement programme.

**Experiment 2 — Route J transfer penalty characterisation (HIGHEST PRIORITY)**
Transfer CVD/Pt graphene to: (a) SCD(111) bonded via 5/10/20 nm SiO₂ interlayers; (b) P(VDF-TrFE) ferroelectric dry transfer; (c) hBN hot pick-up reference. Measure Hall mobility and carrier density. Validates or eliminates Route J as a development bridge; resolves whether SCD(111) substrate infrastructure investment is justified before Route F process maturity.

**Experiment 3 — Raman strain–doping decomposition**
Develop and validate a Raman protocol separating compressive strain (G-peak blue-shift) from carrier doping (2D-peak red-shift / asymmetry) at each integration stage. Converts Raman from a heuristic tool into a quantitative prior-updating instrument. No current ISO/IEC standard covers this parameter for photonic device integration.

**Experiment 4 — Spatial carrier density mapping**
KPFM or scanning photocurrent microscopy over ≥50×50 µm² at sub-micron resolution. Measure spatial correlation length L_corr of carrier density fluctuations at each integration stage including post-ferroelectric switching cycle. Determines whether array is in the averaging regime (L_corr ≪ cell pitch → global calibration sufficient) or mesoscopic disorder regime (L_corr ~ cell pitch → single-crystal substrates required).

---

## Three-phase practical strategy

1. **Now:** Route I (CVD/Pt → hBN) for early device demonstrations — available today, 38.6% per-cell pass probability
2. **Bridge:** Route J (CVD/Pt → SCD(111)) when diamond substrate infrastructure is in place — *pending Experiment 2 validation*
3. **Long-term:** Route F (SCD(111) Ni+H-term in-place graphitization) when 3,500–4,500 cm²/V·s crossover threshold is cleared

No redesign of the photonic stack is required between phases.

---

## Key limitations

- The Goldilocks window is derived from a device architecture with no experimental prototype. Thresholds are falsifiable hypotheses, not validated specifications.
- Route F mobility prior anchored to arXiv 2022 — single-point failure risk. Experiment 1 is the direct test.
- Route J transfer penalty is an order-of-magnitude estimate with no direct experimental measurement.
- Correlation matrix values are physically motivated but not fitted to experimental data.
- The array yield model (P_array ~ P_cell^N²) assumes independent cells — upper-bound model. Experiment 4 provides the correlation-aware replacement.
- The author does not have access to the experimental infrastructure required for Experiments 1–4. This is a call to the field.

---

## Repository contents

```
graphene-photonic-memory-routes/
├── paper/
│   └── Paper4_v9_1_Haaland_2026.pdf      # Current version
├── simulation/
│   └── priors.json                         # Prior parameter table (Table 3)
│   └── monte_carlo.py                      # Simulation code (forthcoming)
├── index.html                              # Project landing page
├── README.md
└── LICENSE.md                             # CC BY 4.0
```

Simulation code and full prior parameter files will be released as supplementary material upon journal submission.

---

## Relation to prior work

This is Paper 4 of a four-paper series on non-volatile photonic weight memory for AI accelerators centred on graphene-ferroelectric hybrid architectures (Al:HfO₂, graphene integration, MVM tile design).

Related repository: [graphene-photonic-memory-routes verifier tool](https://bluebflatminor.github.io) — Monte Carlo route verifier (HTML/JS, interactive).

---

## Key literature anchors

| Reference | Role in framework |
|---|---|
| Xu et al., Nature Comms 16, 2025 | Pockels photonic memory, 6 optical states, 10⁷-cycle endurance |
| Chen et al., Nanoscale, July 2025 | Graphene-HZO endurance >10⁸ cycles, domain engineering mechanism |
| Patil et al., Small, Oct 2025 | Graphene planar electrode on HZO, sub-fJ switching, closest geometry |
| Banszerus et al., Sci. Adv. 1, 2015 | Route I hBN encapsulation transfer anchor |
| Gao et al., Nature Comms 3, 2012 | CVD/Pt growth prior anchor |
| Zhang et al., Adv. Mater. 37(50), 2025 | P(VDF-TrFE) ferroelectric dry transfer |

---

## Citation

```bibtex
@misc{haaland2026graphene,
  title   = {Graphene Integration Route Selection for Ferroelectric Photonic Memory:
             A Bayesian Framework for Pre-Experimental Decision-Making},
  author  = {Haaland, Nils},
  year    = {2026},
  month   = {May},
  version = {9.1},
  note    = {Solbakken Research Initiative, Omaha. Independent preprint.},
  url     = {https://github.com/bluebflatminor/graphene-photonic-memory-routes}
}
```

---

## License

[CC BY 4.0](LICENSE.md) — open, attribution required. See LICENSE.md for citation format.
