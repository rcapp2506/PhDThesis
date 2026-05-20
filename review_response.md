# Review-response document

Roberto Cappuccio, Università di Siena, Ciclo XXXVII
Doctoral thesis: *"New Perspectives on Quantum Technologies: Progress on Quantum Sensing and Quantum Computation"*

This document collates the reviewers' comments and describes, for each one, the action taken in the revised manuscript. Two referee reports are addressed: the technical report by Prof. Gatti (comments G1–G17) and the formal report by Prof. Signorelli (comments S1–S7). All comments are closed in the current revision (HEAD `ffd4843`, 181 pages, 0 undefined references, 0 undefined citations, build stable after four pdflatex passes).

## Summary table

| Code | Topic | Status | Where the change lives |
|---|---|---|---|
| G1 | IQM Emerald hardware validation | Closed | Cap. 3 §Hardware validation; commit `e13dd8c` |
| G2 | Typo cluster | Closed | Cap. 2; commit `0f5ddf5` |
| G3 | Typo cluster | Closed | Cap. 2; commit `0f5ddf5` |
| G6 | Hardware chapter wording | Closed | Cap. 2; commit `dc5f826` |
| G7 | Eq. 2.39 transverse vs. longitudinal | Closed | Cap. 2 §Two-qubit baseline; commits `5c279da`, `6630487`, `621d7d1`, `a346650`, `56875b6` |
| G8 | Eq. 2.57 X = HZH | Closed | Cap. 2; commit `dc5f826` |
| G9 | $\tan\delta$ at 300 GHz | Closed | Cap. 2 §Decoherence Analysis; commit `dc5f826` |
| G10 | $I_c$ 30 µA vs. 1.5 µA | Closed | Cap. 2; commit `f256500` |
| G11 | Notation issue | Closed | Cap. 4 §Sensing; commits `0f5ddf5`, `a472751` |
| G12 | Sensing chapter attribution | Closed | Cap. 4; commit `a472751` |
| G13 | Photon rate $10^2$ not $10^{-9}$ | Closed | Cap. 4; commit `e5c6230` |
| G14 | $\Gamma_{\text{thermal}}$ vs. rate $\times N_{\text{atoms}}$ | Closed | Cap. 4 §Thermal background; commit `e5c6230` |
| G15 | Hardware closeout | Closed | Cap. 2; commit `dc5f826` |
| G16 | Hardware closeout | Closed | Cap. 2; commit `dc5f826` |
| G17 | Backaction atoms on cavity | Closed | Cap. 4 §Backaction; commit `0b01b72` (`eta_eff(N)` figure added) |
| S1 | Pedagogical depth | Closed | Distributed across Cap. 3, Cap. 4, Cap. 5; commits `aa63192`, `94d4da9` |
| S2 | Pedagogical hardware/algorithms | Closed | Cap. 2, Cap. 3; commit `64d8ebf` |
| S3 | Bullet refactor | Closed | Cap. 2 (75 → 24 bullet/enumerate blocks); commits `50e9355`, `892a7bc` |
| S4 | Pedagogical closeout | Closed | Cap. 2, Cap. 3; commits `64d8ebf`, `03e2599` |
| S5 | Conclusions from captions to body | Closed | Cap. 2, Cap. 3; commits `64d8ebf`, `03e2599` |
| S6 | Global-style sweep | Closed | All chapters; commits `22e5915`, `db46bd8` |
| S7 | Global-style sweep | Closed | All chapters; commits `22e5915`, `db46bd8`, `03e2599` |

## Detail by comment

### Gatti technical report

**G1 — IQM Emerald hardware validation.** A dedicated subsection in Cap. 3 reports the IQM Emerald hardware validation campaign, including the calibration loop, the noise model with the dominant gate-error contributions, and the comparison between the noise-free simulator, noisy simulator, and the IQM Emerald backend. The validation confirms that the C16–Q64 QCNN architecture preserves binary-classification accuracy on EuroSAT under the noise profile of the device, within the statistical fluctuations of finite-shot estimation.

**G2, G3 — Typo cluster.** Resolved through dedicated proofreading passes, with the table overflow issues in Tab. 2.19 and Tab. 2.23 fixed in the same patch.

**G6 — Hardware chapter wording.** Several wording issues throughout Cap. 2 were addressed in the hardware closeout patch.

**G7 — Eq. 2.39 transverse vs. longitudinal.** This was the most articulated technical comment. The reviewer pointed out that the original formula $J = (g_1 g_2 / 2)(1/\Delta_1 + 1/\Delta_2)$ was being attributed to the cross-Kerr longitudinal $\zeta$ coupling, whereas it is the coefficient of the transverse exchange $\sigma_+^{(1)}\sigma_-^{(2)} + \text{h.c.}$ The closure required (i) deriving the Schrieffer-Wolff transformation explicitly using sympy and qutip; (ii) regenerating the figures from `qh_hardware/` with corrected dispersive shift numerics; (iii) refactoring the inline section structure of the chapter; (iv) tightening the figure captions. The chain of commits is documented above.

**G8 — Eq. 2.57 $X = HZH$.** The Hadamard-conjugate identity for the Pauli-X operator was corrected.

**G9 — $\tan\delta$ at 300 GHz.** The dielectric loss tangent in the decoherence budget at 300 GHz was rederived and the corresponding entry in the $T_1$ breakdown table updated.

**G10 — $I_c$ 30 µA vs. 1.5 µA.** The critical current value used in the cavity-QED simulation was inconsistent across two paragraphs (30 µA in the figure caption, 1.5 µA in the inline derivation). The correct value was traced back to the source script in `qh_hardware`, and the manuscript was aligned to it.

**G11 — Notation issue.** Resolved through notation cleanup in Cap. 4.

**G12 — Sensing chapter attribution.** Several bibliographic attributions in the sensing chapter were corrected.

**G13 — Photon rate $10^2$, not $10^{-9}$.** The conversion from cavity photon density to photon rate was off by 11 orders of magnitude (a unit error in the conversion factor). The correct rate $\sim 10^2$ s$^{-1}$ was confirmed by the source script and propagated to the manuscript, with the corresponding figures regenerated.

**G14 — $\Gamma_{\text{thermal}}$ vs. rate $\times N_{\text{atoms}}$.** The thermal background expression was reformulated from "rate per atom times $N_{\text{atoms}}$" to a single $\Gamma_{\text{thermal}}$ scaling with $N_{\text{atoms}}$ implicitly, in line with the way the figure pipeline implements the calculation.

**G15, G16 — Hardware closeout.** Resolved in the hardware closeout commit `dc5f826`.

**G17 — Backaction atoms on cavity.** A new figure was added showing the backaction transparency $\eta_{\text{eff}}(N)$ as a function of the atomic ensemble size, with the spatial signature emphasis and the CARRACK/Graham positioning explicit.

### Signorelli formal report

**S1 — Pedagogical depth.** The original suggestion was to add a dedicated background chapter. Following the project decision to preserve the chapter structure (Cap. 1 Intro → Cap. 2 Hardware → Cap. 3 Algorithms → Cap. 4 Sensing → Cap. 5 Conclusions), pedagogical depth was added through distributed explanatory paragraphs within the existing chapters: a Preliminaries subsection and a decoherence taxonomy in Cap. 3, expanded readout technique descriptions in Cap. 4, distributed pedagogy in Cap. 5.

**S2 — Pedagogical hardware/algorithms.** Pedagogical paragraphs added to the hardware and algorithms chapters.

**S3 — Bullet refactor.** The original Cap. 2 used 75 bullet/enumerate blocks; these were reduced to 24, with the remaining ones reserved for genuinely enumerable content (taxonomies, lists of equations, prerequisites). The overall flow was converted to prose.

**S4 — Pedagogical closeout.** Pedagogical wording closure in Cap. 2 and Cap. 3.

**S5 — Conclusions from captions to body.** Several concluding statements that originally appeared only in figure captions were moved into the body text of the corresponding sections, so that the narrative is self-contained without needing to read the captions.

**S6, S7 — Global-style sweep.** Acronym expansion (NV, ODMR, EIT, SQUID, JJ, KID, ADMX, HAYSTAC, TRL); targeted style sweep across all chapters; final coherence pass.

## Additional revisions not requested by reviewers

In the course of preparing the post-defense revision, two additional consistency issues were identified and addressed; they are listed here for completeness.

### QP/4K narrative coherence (Cap. 2 §Decoherence Analysis, §Summary, §P3 prerequisite)

The quasiparticle-induced $T_1$ derivation in Cap. 2 was originally written using the aluminum gap $\Delta_{\text{sc}} = 180\,\mu$eV at $T = 4$ K, which is internally inconsistent because aluminum is in the normal state above its $T_c \approx 1.2$ K. The formula yielded $n_{qp}(4\,\text{K}) = 2.06$, an unphysical value $>1$ that is mathematical evidence of the formula being applied outside its regime of validity.

The derivation was rewritten using the NbN gap $\Delta_{\text{NbN}} \approx 2.8$ meV consistent with the rest of the chapter (where the NbN/AlN/NbN platform is the design choice). The corrected derivation yields:

$$x_{qp}^{th}(4\,\text{K}) \simeq 2.7 \times 10^{-4}, \qquad T_1^{qp,\text{intrinsic}} \simeq 20\,\text{ns}$$

via the Catelani-Schoelkopf-Devoret-Glazman formula. This intrinsic value is three orders of magnitude shorter than the operational coherence target $T_1^{qp} \simeq 49\,\mu$s required to support the gate fidelities of the algorithms of Cap. 3. To close the gap, a new `\paragraph{Engineering pathway to the operational target}` was added immediately after, identifying:

- gap-asymmetric junctions [Marchegiani 2022], which exponentially suppress the uphill tunneling channel in the non-equilibrium regime;
- normal-metal traps on both electrodes [Riwar 2016], which lower $x_{qp}$ in the steady state of the kinetic balance.

A target-driven kinetic analysis (numerically verified, with all source scripts archived in the qh_hardware repository under `audit_qp_kinetics/`) shows that $T_1^{qp} \simeq 49\,\mu$s is consistent with bilateral normal-metal traps at a rate $s \sim 10^7$ s$^{-1}$ on symmetric NbN/AlN/NbN junctions, approximately one order of magnitude beyond the rates demonstrated in aluminum-based devices. Gap engineering combined with trap engineering does not relax this requirement at 4 K because of the larger thermal quasiparticle population on the low-gap electrode; this point is derived explicitly in the kinetic appendix kept in the repository (`chapters/appendix_quasiparticles.tex`, intentionally not included from `main.tex` in the delivered version).

The narrative coherence with this revision was verified across:

- §Decoherence Analysis (the new derivation and engineering pathway);
- §Summary of Findings (the total $T_1 = 15.3\,\mu$s is now explicitly tagged as assuming the trap-engineered configuration);
- §Technological prerequisites P3 (the previous wording conflating direct photon pair-breaking with thermal QP tunneling was disambiguated; the two channels are now described as distinct phenomena with distinct mitigation paths).

Commits: `cf20604` (Engineering pathway paragraph), `272dd76` (Al→NbN derivation correction), `42b44e1` (parenthetical removed), `ef286ba` (Summary tagging), `4615ae8` (P3 disambiguation), `ffd4843` (bibliography merge).

### Bibliography integrity

The bibliography file `chapters/full_references_checked.bib` now contains 244 entries (218 `@article` + others), all of which are correctly resolved against the 147 `\cite` calls in the included sources. The seven QP-kinetics references appended in commit `ffd4843` (Rothwarf 1967, Kaplan 1976, Riwar 2016, Hosseinkhani 2017, Marchegiani 2022, Demsar 2003, Kamenov 2024) were checked against the existing keys to rule out collisions before merging.

## Build verification

The current state of the manuscript was built with the following pipeline:

```
pdflatex main.tex     (pass 1: 168 pages, references pending)
bibtex main           (cite resolution)
pdflatex main.tex     (pass 2: 181 pages)
pdflatex main.tex     (pass 3: 181 pages, label "may have changed" warning)
pdflatex main.tex     (pass 4: 181 pages, stable)
```

Final build statistics:

- 181 pages
- 0 errors
- 0 undefined citations
- 0 undefined references
- 0 multiply-defined labels
- 23 overfull hbox (largest 36.2 pt, all cosmetic)
- 24 underfull hbox (cosmetic)

The output file is `main.pdf`, 15.2 MB.

## Supporting material

The numerical audits supporting the revisions are archived in the companion repository `qh_hardware`:

- `audit_g7/` — Schrieffer-Wolff derivation for G7 (sympy + qutip)
- `audit_qp_kinetics/` — QP kinetic-balance analysis supporting the QP/4K narrative coherence revision; closed-form solutions verified against numerical ODE integration

Both folders include a README, the scripts that produced the figures, and the figures themselves in both PDF (for thesis inclusion) and PNG (for quick inspection) formats.
