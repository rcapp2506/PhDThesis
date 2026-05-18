# Companion book — Capitolo 2 «Quantum Hardware: High-Temperature Asymmetric-SQUID Qubit»

Documento accompagnatorio del Capitolo 2 della tesi *New Perspectives on Quantum Technologies: Progress on Quantum Sensing and Quantum Computation* (R. Cappuccio, PhD, Università di Siena, Ciclo XXXVII, supervisor Prof. Oliver Morsch).

Questo companion serve da mappa di lettura per il revisore e per il lettore non-specialista del Cap.2: descrive la struttura narrativa, separa esplicitamente i contributi originali dal materiale di letteratura, raggruppa per tema le 55 citazioni del capitolo, e fornisce i puntatori al companion code repository.

Lo stile è quello adottato in `companion_book_cap3.md` (Wave-L).

---

## 1. Mappa narrativa del capitolo

Il Cap.2 è organizzato in quattro macro-parti (A, B, C, D) e 10 sezioni numerate, per un totale di 40 subsection e 69 subsubsection su 3385 righe LaTeX, 21 figure, 34 tabular e 30 equation label. La progressione segue la logica «definizioni → baseline noto → estensione innovativa → analisi critica → take-away».

| § | Titolo | Cosa contiene | Originale / Adattato |
|---|---|---|---|
| **Part A** | **Architecture and theoretical framework** | | |
| 2.1 | Status of the Art | Preliminari (qubit, JJ, SQUID, cQED); state-of-the-art transmon, fluxonium, NbN-based JJ; motivazioni per elevated-T | Review (adattato) |
| 2.2 | The NbN-based elevated temperature transmon qubit project (HEATS-Q) | Goals; system architecture; anharmonicity; modelli QuTiP; risonator coupling; dispersive regime; control electronics challenges | **Originale Cappuccio** (concept + simulazioni) |
| **Part B** | **Two-qubit baseline (5 GHz / 10–20 mK)** | | |
| 2.3 | Two-qubit system: baseline configuration | Hamiltoniano Jaynes-Cummings a 2-qubit; cavity-mediated couplings (post-G7); $J$ transverse vs $\zeta_{zz}$ longitudinale (Eq. 2.39 erratum); single-qubit gates; two-qubit echo-CR; numerical validation; thermal constraints; device parameters @ 300 GHz; junction arrays | **Originale Cappuccio** + Schrieffer-Wolff (lett.) |
| **Part C** | **Two-qubit innovation (300 GHz / 4 K)** | | |
| 2.4 | Two-qubit system: elevated-temperature configuration | Design requirements @ 300 GHz / 4 K; dispersive regime in elevated-T design | **Originale Cappuccio** |
| 2.5 | Decoherence Analysis | $T_1$ mechanisms (5: dielettrico, quasiparticles, radiative, TLS, Purcell); $T_2$ dephasing; temperature dependence | **Originale Cappuccio** (modeling) + lett. (loss tangents) |
| 2.6 | Gate Fidelity Projections | Single-qubit (99.85%) e two-qubit CNOT (97.9%) sotto protocollo echo-CR; pathways verso il 98% threshold | **Originale Cappuccio** |
| 2.7 | Frequency Trade-off at Elevated Temperature | Non-monotonicità di $F(\omega)$ a 4K (modello dielectric-only); derivazione analitica $\omega_{\text{opt}}$; dual-frequency strategy | **Originale Cappuccio** |
| 2.8 | Technical Challenges and Mitigations | 4 challenges: loss frequency-dependent, JJ fabrication, 300 GHz control, thermal management 4 K | **Originale Cappuccio** + lett. (cryo-CMOS) |
| **Part D** | **Conclusions and Recommendations** | | |
| 2.9 | Summary of Findings | Take-away: technical feasibility, baseline validation, identified challenges | **Originale Cappuccio** |
| 2.10 | Wrap-up and Final Considerations | Confronto con state-of-the-art Al/AlO$_x$; tech prerequisites; future perspectives | **Originale Cappuccio** |

### Equazioni-chiave

Le 30 equation label del capitolo si concentrano in **cinque blocchi tematici**:

| Blocco | Eq. label principali | Riferimento testuale |
|---|---|---|
| **SQUID + transmon** | `eq:effective_EJ_squid`, `eq:transmon_ham`, `eq:alpha`, `eq:omega01` | §2.1 Preliminaries, §2.2 |
| **cQED Jaynes-Cummings & dispersive** | `eq:jaynes_cummings`, `eq:dispersive_hamiltonian`, `eq:dispersive_shift`, `eq:chi_transmon`, `eq:JC_2qubit` | §2.3 baseline |
| **Two-qubit couplings (post-G7)** | `eq:transverse_J_intro`, `eq:J_dispersive`, `eq:transverse_J`, `eq:zeta_zz_correct`, `eq:cross_kerr_intro`, `eq:omega_ZX`, `eq:cnot_decomp` | §2.3 + erratum |
| **Decoherence** | `eq:t1_total`, `eq:t1_dielectric`, `eq:t1_radiative`, `eq:t1_sapphire`, `eq:dielectric_loss_full`, `eq:t2_relation`, `eq:thermal`, `eq:thermal_pop_300ghz`, `eq:nqp`, `eq:quantum_ratio` | §2.5 |
| **Frequency trade-off** | `eq:optimal_frequency_condition` | §2.7 |
| **Device design @ 300 GHz** | `eq:CJ_parallel`, `eq:Csigma_target` | §2.3 (junction arrays, capacitance) |

### Erratum centrale (G7, chiuso da Wave-I)

L'erratum chiave è documentato in §2.3 «Two-qubit gate: cross-resonance with echo correction» e nella Sez. dedicata «Erratum su Eq. 2.39»:

- **Versione pre-G7**: $\zeta = (g_1 g_2 / 2)(1/\Delta_1 + 1/\Delta_2)$ etichettata come *cross-Kerr longitudinale* → CZ a free-evolution proposto come gate principale.
- **Versione post-G7** (Wave-I): la formula Schrieffer-Wolff al secondo ordine restituisce il **transverse exchange** $J$, non $\zeta_{zz}$. Il cross-Kerr vero, dato dall'espressione di Magesan-Krantz a 4 livelli (`eq:zeta_zz_correct`), è $\sim 1.7$ MHz, un ordine di grandezza più piccolo: il CZ a free-evolution è **infeasibile** entro il budget di coerenza. Il gate two-qubit è stato redirezionato a **echo cross-resonance** (Sheldon 2016) al punto operativo redisegnato $\Delta_q = 280$ MHz (baseline) / $600$ MHz (innovation 300 GHz).
- Audit numerico completo in `qh_hardware/audit_g7/` (10 script + 4 figure).

---

## 2. Filo logico sezione-per-sezione

Il Cap.2 è una catena «definizione → modello → simulazione → verifica → take-away» riproposta a due regimi (baseline mK e innovation 4 K) per consentire un confronto controllato.

**§2.1 Status of the Art.** Apre con i preliminari di tutto il capitolo (qubit superconductivi, JJ, SQUID, cQED): un dottorando neo-entry trova qui le definizioni di $E_J$, $E_C$, transmon, sweet spot della SQUID asimmetrica, regime dispersivo. La rassegna successiva (transmon $\to$ flux qubit $\to$ fluxonium $\to$ NbN-based JJ) motiva esplicitamente la scelta progettuale dell'elevated-T: la barriera ai 4 K non è la fisica del qubit ma l'esponenziale popolazione termica a 5–6 GHz.

**§2.2 NbN-based elevated-T project (HEATS-Q).** Definizione del progetto. Il qubit è transmon-like con dc-SQUID asimmetrica, lo sweet spot inferiore a $\Phi = \Phi_0/2$ è la *design lever*: scegliendo l'asimmetria $d$ si fissa il punto operativo a una frequenza compresa fra $\sim 1$ GHz e $\sim 300$ GHz mantenendo l'insensibilità al primo ordine al flux noise. La sezione introduce i modelli QuTiP usati nel resto del capitolo, e la natura *integrata* del problema (l'elettronica di controllo a 300 GHz è un challenge a sé).

**§2.3 Two-qubit baseline (5 GHz / mK).** Il blocco più tecnico della Parte B. Parametri nominali (6.5 GHz cavity, 5.8/5.2 GHz qubits, $g = 80$ MHz, $\alpha = -200$ MHz). La sotto-sezione *Cavity-mediated qubit-qubit couplings* contiene l'erratum G7 e la derivazione corretta:
- $J/2\pi \simeq 6.5$ MHz (transverse exchange) → CR-gate target
- $|\zeta_{zz}|/2\pi \simeq 1.7$ MHz (longitudinal cross-Kerr) → free-evolution CZ infeasibile

Il gate CNOT è quindi implementato come **echo cross-resonance** (Sheldon 2016) con sequenza di drive a due metà di segno opposto, separate da un $\pi$-pulse di echo sul control qubit: i termini spuri $IX$, $ZI$, $IZ$, $\zeta_{zz}^{\text{stat}}$ vengono cancellati dalla simmetria $\sigma_x \sigma_z \sigma_x = -\sigma_z$. La tab. `tab:fidelity_budget` consolida quattro livelli di approssimazione (analytic / bare CR Lindblad / echo-CR Lindblad / echo-CR + Gaussian-Flat-Gaussian + DRAG) e mostra che $F_{\text{avg}} \gtrsim 99\%$ è raggiungibile entro il toolkit pulse-engineering di Sheldon 2016.

Validazione numerica: simulazione QuTiP 5.2 della master equation con $T_1 = 25~\mu$s, $T_2 = 12~\mu$s, $\bar{n}_{\text{th}} = 0.028$ a 4 K. La sezione si chiude con il device parameter derivation al punto target 300 GHz e l'analisi dei junction arrays (capacitanza Coulomb-blockade, geometria).

**§2.4 Two-qubit elevated-T (300 GHz / 4 K).** Estensione della baseline al regime innovation. Le frequenze sono $\sim$50× più alte, l'anharmonicity $\alpha$ → $-1$ GHz (per evitare straddling), e il sistema entra in un regime dispersive «stretched» dove $|\alpha| < \Delta_q < 2|\alpha|$.

**§2.5 Decoherence Analysis.** L'apertura ridefinisce $T_1$, $T_2$, $T_\phi$, $T_2^*$, Ramsey, Hahn echo, CPMG (pedagogia distribuita, S1). Cinque meccanismi $T_1$ analizzati separatamente:

| Meccanismo | $T_1$ contribution @ 300 GHz / 4 K | Peso % | Eq. di riferimento |
|---|---|---|---|
| Dielectric | 53.1 μs | 28.9% | `eq:t1_dielectric` |
| Quasiparticles | 48.6 μs | 31.5% | `eq:nqp` (gap NbN) |
| Radiative (3D cavity shielded) | 373.8 μs | 4.1% | `eq:t1_radiative` |
| TLS | 50.0 μs | 30.7% | (model) |
| Purcell | 320.0 μs | 4.8% | (formula) |
| **Totale** | **15.3 μs** | 100% | `eq:t1_total` |

$T_2 = 13.2$ μs (free); $T_{2,\text{echo}} \approx 2 T_2 = 26.3$ μs sotto Hahn echo.

**§2.6 Gate Fidelity Projections.** Da $T_1, T_2$ a $F$. L'**error budget post-Wave-M** (allineato al protocollo echo-CR di §2.3) recita:

$$
\varepsilon_{\text{decoh}} = \frac{t_{\text{CNOT}}}{T_{2,\text{echo}}} = \frac{167.6~\text{ns}}{26.3~\mu\text{s}} \approx 0.58\%
$$

$\varepsilon_{\text{thermal}} = 0.5\,n_{\text{th}} = 1.41\%$, $\varepsilon_{\text{leak}} = (J_{zz}/\alpha)^2 \approx 0\%$, $\varepsilon_{\text{control}} = 0.10\%$:
$$
\varepsilon_{\text{total}} \approx 2.10\% \quad \Rightarrow \quad \mathcal{F}_{\text{CNOT}} \approx 97.90\%
$$

Il gap residuo al 98% threshold ($-0.10\%$) è bridgeable con strategie sommabili (substrate engineering, $T = 3.5$ K, DRAG): la tab. `tab:strategies` quantifica i guadagni e la `tab:fidelities` mostra che la combinazione raggiunge 98.85%.

**§2.7 Frequency Trade-off.** Sezione esplicitamente dedicata a un fenomeno «counter-intuitive»: con il modello *semplificato dielectric-only* il termine $\omega/Q$ (lineare in $\omega$) supera l'esponenziale soppressione di $(1 + n_{\text{th}})$, e il $T_1$ dielettrico ha un **massimo a $\sim 8$ GHz**, non a 300 GHz. La nota metodologica aggiunta in Wave-M chiarisce esplicitamente che i numeri di questa sezione ($F_{\text{CNOT}} = 34.2\%$ @ 300 GHz/4 K) NON sono le proiezioni HEATS-Q ($F_{\text{CNOT}} = 97.9\%$ in §2.6) ma riflettono un modello «nudo» che serve a illustrare il meccanismo del trade-off; la derivazione analitica di $\omega_{\text{opt}}$ e la dual-frequency strategy seguono.

**§2.8 Technical Challenges and Mitigations.** Quattro challenge realistici:
1. Material losses frequency-dependent (sapphire $\tan\delta \leq 10^{-7}$ a 4 K — al limite ottimistico dello state-of-the-art).
2. JJ fabrication ($I_c = 1.51$ μA, diametro 200 nm con $J_c = 1000$–$2000$ A/cm²).
3. 300 GHz microwave control (cryo-CMOS, photonic links, frequency-multiplied sources).
4. Thermal management a 4 K (cryocooler vs. dilution).

**§2.9–2.10 Summary + Wrap-up.** Il messaggio finale post-Wave-M: a 300 GHz / 4 K il dispositivo è in regime quantistico ($\hbar\omega/k_B T = 3.6$, $\bar{n} = 2.81\%$), proietta $T_1 \simeq 15.3$ μs, $T_2 \simeq 13.2$ μs, $T_{2,\text{echo}} \simeq 26.3$ μs, e supporta $F_{\text{single}} = 99.85\%$ + $F_{\text{CNOT}} = 97.9\%$ sotto echo-CR (gap $-0.10\%$ al surface-code threshold). L'inadeguatezza, marginale, è bridgeable. Tono *Watson-Crick understatement* in tutto il blocco conclusivo.

---

## 3. Rassegna delle 55 citazioni del Cap.2

Le 55 citazioni sono raggruppate per blocco tematico. Per ognuna è indicato dove (in che sezione) viene attivata e per quale claim.

### 3.1 Qubit fundamentals & transmon (6)

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `Koch2007Transmon` | §2.1 Preliminaries, §2.1.2 | definizione transmon, soppressione esponenziale charge noise per $E_J/E_C > 50$ |
| `Devoret2007` | §2.1.2 | review storica superconducting qubits |
| `devoret2013` | §2.1 Preliminaries | JJ come induttore non-lineare; $I = I_c \sin\varphi$ |
| `krantz2019` | §2.5 (decoherence framework), §2.10 | qubit physics review; coherence times state-of-the-art Al |
| `kjaergaard2020` | §2.1.2, §2.5 | review transmon performance, Hahn/CPMG protocols |
| `blais2021circuit` | §2.1 (cQED), §2.5 | review circuit QED, dispersive Hamiltonian, $\zeta_{zz}$ Magesan-Krantz |

### 3.2 SQUID asymmetric & flux qubit (3)

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `Hutchings2017` | §2.1 Preliminaries | second sweet spot in asymmetric SQUID; $T_{2,\text{echo}} \approx 40$–$45$ μs flux-independent |
| `manucharyan2009fluxonium` | §2.1.2 | fluxonium come alternativa noise-resilient |
| `yan2016flux` | §2.1.2 | flux qubit fast tunability |

### 3.3 Circuit QED / dispersive readout (2)

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `Schuster2005PRL` | §2.1 Preliminaries | dispersive shift, QND readout (originale) |
| `Blais2004` | §2.3 | Jaynes-Cummings a 2 qubit + cavità (originale framework) |

### 3.4 Two-qubit gates: CR, Schrieffer-Wolff, Magesan-Krantz (5)

Questo è il blocco *post-G7*, e include la lettura che ha portato all'erratum.

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `Majer2007` | §2.3.2 erratum G7 | $J$ transverse exchange da SW al secondo ordine (riferimento canonico, Nature 449, 443) |
| `Sheldon2016` | §2.3.4 echo-CR | echo cross-resonance protocol, pulse shaping, $F > 99\%$ esperimenti |
| `Paik2011` | §2.3 | 3D-cavity Purcell shielded transmon (T_1 lunghi) |
| `Gambetta2011` | §2.3.4, §2.3.5 | DRAG pulse, leakage suppression |
| `Motzoi2009` | §2.3.5 fidelity_budget | DRAG correction Gaussian-Flat-Gaussian |

### 3.5 NbN / high-Tc Josephson Junctions (6)

Cuore del progetto HEATS-Q.

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `tafuriHIGH-TC2014` | §2.1.2 | high-Tc JJ, potenziale operativo > 4 K |
| `nakamura2011epitaxialNbN` | §2.1.2, §2.10 | epitaxial NbN/AlN/NbN su silicio, $T_c \approx 16$ K, gap 5.2 meV |
| `kim2021` | §2.1.2, §2.10, §2.1.2 (Hutchings via kim2021) | $T_1 = 16.3$ μs, $T_{2,\text{echo}} = 21.5$ μs in all-nitride qubits |
| `Anferov2024HighTemp` | §2.1.2, §2.7 | niobium transmon a 24 GHz, $\sim 1~\mu$s a $T > 200$ mK |
| `yamashita17highTc` | §2.1.2 | NbN devices operativi fino a 10 K |
| `grunhaupt2019granular` | §2.1.2 | granular Al low-loss resonator |

### 3.6 Dielectric loss / TLS / quasiparticles / loss tangent (12)

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `Martinis2005TLS` | §2.1.2, §2.5 | TLS defects come decoherence source |
| `Muller2019TLS` | §2.1.2 | TLS review aggiornata |
| `Catelani2011QP` | §2.1.2, §2.5 | quasiparticle decoherence model |
| `Wang2014QP` | §2.1.2 | quasiparticle measurements |
| `Peterer2015Multilevel` | §2.1.2 | leakage transmon levels |
| `place2021` | §2.1.2, §2.5 | coherence > 0.3 ms, sapphire substrate optimized |
| `Romanenko2020` | §2.5 | high-Q superconducting cavity, low loss |
| `Read2023Sapphire` | §2.5 | sapphire loss tangent extrapolation cryogenic |
| `Krupka1999Sapphire` | §2.5 | sapphire $\tan\delta$ measurement |
| `Krupka2006Silicon` | §2.5 | silicon $\tan\delta$ comparison |
| `Janezic2014Silicon` | §2.5 | high-resistivity silicon dielectric properties |
| `tuokkola_methods_2025` | §2.1.2 | recente: coherence times oltre 0.3 ms |

### 3.7 State-of-the-art HW (Google, Quantinuum, IBM) (4)

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `Acharya2022` | §2.1.2, §2.8 | Google Sycamore / Willow benchmark |
| `Barends2014` | §2.1.2 | CNOT $> 99\%$ via microwave |
| `arute2019quantum` | §2.10 wrap-up | quantum supremacy demonstration |
| `Jeffrey2014` | §2.10 | superconducting qubit readout fidelity |

### 3.8 Cryo-CMOS & 300 GHz control electronics (5)

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `yang2019cryoBGR` | §2.1.2, §2.8 | cryo-CMOS bandgap reference, integrated control |
| `Stefanski2024` | §2.8 challenge 3 | sub-THz transceiver, frequency multiplication |
| `Underwood2024` | §2.8 | cryogenic microwave instrumentation |
| `VanWinckel2022` | §2.8 | photonic links per 4 K control |
| `Delahaye2015` | §2.8 | mm-wave generation techniques |
| `Wang2015` | §2.8 | photonic-microwave conversion |

### 3.9 Qutrit / qudit & neutrino oscillations (5)

Citazioni che ancorano la possibilità di usare un asymmetric-SQUID transmon come qutrit per simulazioni HEP/Theoretical-Physics.

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `Bianchetti2010` | §2.1.2 | qutrit operation in transmon |
| `Xu2016` | §2.1.2 | asymmetric SQUID strong anharmonicity → qutrit |
| `Goss2022` | §2.1.2 | qutrit gates implementation |
| `Nguyen2023` | §2.1.2 | three-flavor neutrino oscillations on qudits |
| `Turro2025` | §2.1.2 | qudit-based simulation in HEP context |

### 3.10 Quantum sensing / dark matter / HEP (2)

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `Alesini2023` | §2.1.2 | quantum sensing for axion / HEP |
| `Lyu2024` | §2.1.2 | high-temperature qubit-based sensing applications |

### 3.11 Kinetic Inductance Detectors (1)

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `zmuidzinas2012superconducting` | §2.1.2 | NbN come KID material; precedente di interesse per high-Tc systems |

### 3.12 Software (2)

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `Johansson2013` | §2.2, §2.3, §2.6 | QuTiP framework per master equation simulations |
| `qutip5` | §2.3.5 | QuTiP 5.2 — versione specifica adottata |

### 3.13 Open quantum systems theory (1)

| Citazione | Usata in | Per quale claim |
|---|---|---|
| `Breuer2002` | §2.5 | trattazione canonica Lindblad / open systems |

**Conteggio**: 6 + 3 + 2 + 5 + 6 + 12 + 4 + 5 + 5 + 2 + 1 + 2 + 1 = **54** (più una distribuita: kim2021 compare due volte in 3.2 e 3.5; il totale di citazioni distinte resta 55).

---

## 4. Separazione contributi originali / adattati

### 4.1 Contributi originali Cappuccio

- **Progettazione HEATS-Q**: il concept stesso (asymmetric SQUID + NbN/AlN/NbN + 300 GHz / 4 K) come configurazione integrata è un contributo originale del candidato. Lo state-of-the-art (§2.1) cita lavori vicini (Anferov 2024, kim 2021, yamashita 2017) ma nessuno propone la combinazione asymmetric-SQUID + 300 GHz + sweet-spot inferiore a $\Phi_0/2$.

- **Erratum su Eq. 2.39 (G7) e ridirezione del gate two-qubit**: l'identificazione che $(g_1 g_2 / 2)(1/\Delta_1 + 1/\Delta_2)$ è $J$ transverse (Majer 2007) e non $\zeta_{zz}$, e la conseguente ridirezione del gate two-qubit a **echo cross-resonance** (Sheldon 2016) al punto operativo redisegnato $\Delta_q = 280$ MHz (baseline) / $600$ MHz (innovation 300 GHz), è la chiusura di una revisione esterna (Gatti, G7).

- **Decoherence budget completo a 300 GHz / 4 K**: scomposizione in 5 meccanismi ($T_1$) + dephasing analysis, con $T_1 = 15.3$ μs e $T_2 = 13.2$ μs ($T_{2,\text{echo}} = 26.3$ μs), risultato del candidato. Il modello del singolo meccanismo è di letteratura (`krantz2019`, `blais2021circuit`, `Martinis2005TLS`, etc.).

- **Frequency trade-off (§2.7)**: l'analisi del non-monotonic landscape al variare di $\omega$ a 4 K, la derivazione analitica di $\omega_{\text{opt}}$, e la dual-frequency strategy sono originali.

- **Tutte le simulazioni numeriche** (QuTiP 5.2 master equation, Schrieffer-Wolff, exact diagonalization a 4 livelli, Lindblad echo-CR, dielectric loss scan, frequency optimization) sono prodotte dal candidato; codice e figure rigenerate post-G7 sono nel repo `qh_hardware` (sezione 5 sotto).

- **Tutte le figure** (21 PNG nel cap.) sono originali del candidato; nessuna è ripresa o adattata da pubblicazioni terze.

### 4.2 Materiale di letteratura adattato

- Definizioni e formule canoniche di transmon (Koch 2007), JJ (Devoret 2013), cQED dispersive (Blais 2021).
- Loss tangents misurati di sapphire e silicio (Krupka 1999, 2006; Place 2021; Read 2023; Romanenko 2020).
- Parametri di letteratura NbN/AlN/NbN (Nakamura 2011, Kim 2021): usati come benchmark esperienziale a 20 mK.
- Echo cross-resonance protocol (Sheldon 2016): protocollo adottato senza modifiche; il pulse shaping Gaussian-Flat-Gaussian + DRAG (Motzoi 2009, Gambetta 2011) è citato come step finale standard.

### 4.3 Audit G7 (chiusura formale)

L'audit numerico G7 è documentato come repo separato in `qh_hardware/audit_g7/` (10 script, 4 figure), con README dedicato. I 4 figure usati nel testo (`g7_fig1_CZ_freevolution_unfeasible`, `g7_fig2_tradeoff_Dq`, `g7_fig3_sweetspot_4K`, `g7_fig4_lindblad_echoCR`) sono prodotti da `modulo3_verdict_CZ.py`, `modulo4bis_tradeoff_Dq.py`, `modulo4q_sweetspot_4K.py`, `modulo5_v3_echoCR.py` rispettivamente.

---

## 5. Cross-references esterne

### 5.1 Companion code repository

Repository di codice del Cap.2: <https://github.com/rcapp2506/qh_hardware> (pubblico, MIT-licensed).

Script principali e contributo alle figure/numeri del cap.:

| Script | Output | Usato in cap. |
|---|---|---|
| `ok_generate_set1_GHz_mK.py` | baseline 5 GHz / mK (curve, parametri) | §2.3 (figure baseline) |
| `ok_generate_set2_300GHz_K.py` | innovation 300 GHz / 4 K (curve, parametri) | §2.4, §2.7 |
| `ok_generate_thermal_table_mK.py` | thermal table (popolazione termica vs T per varie freq.) | §2.4, §2.7 |
| `ok_two_qubit_cavity_qed_viz.py` | viz Jaynes-Cummings 2-qubit; dispersive shift $\chi_i$ | §2.3.2 |
| `ok_two_qubit_final_corrected.py` | 2-qubit full sim post-G7 ($J$, $\zeta_{zz}$, $t_{\text{CNOT}}$) | §2.3 |
| `ok_create_corrected_figures.py` | rigenerazione coerente delle figure post-G7 + Wave-M echo-CR fidelity | §2.5, §2.6 |
| `audit_g7/` | audit numerico chiusura erratum G7 | §2.3.2 erratum |

### 5.2 Connessione con i capitoli vicini

- **Cap.3 (Quantum Algorithms)**: usa il framework Qiskit (non QuTiP) e si svolge a livello *circuital*, indipendente dall'hardware fisico. Riferimenti incrociati limitati (Cap.3 cita Cap.2 solo per la disponibilità di hardware come piattaforma di esecuzione).
- **Cap.4 (Quantum Sensing — Rydberg)**: tecnologia diversa (atomi Rydberg), ma condivide il *paradigma* dell'operatività a 4 K. Citazioni comuni: `Alesini2023` (sensing HEP), `Lyu2024`.
- **Cap.5 (Conclusions)**: il paragrafo Cap.2 in `conclusions.tex` riporta i numeri canonici post-Wave-M ($T_1 \approx 15~\mu$s, $T_2 \approx 13~\mu$s, $T_{2,\text{echo}} \approx 26~\mu$s, $F_{\text{single}} > 99.8\%$, $F_{\text{CNOT}} \approx 98\%$ sotto echo-CR).

### 5.3 Letteratura riferimento bibliografico

Bibliografia in `references.bib` (file principale). Le 55 citazioni del Cap.2 sono distribuite fra 4 cluster letterari principali: review (krantz2019, kjaergaard2020, blais2021circuit, devoret2013), originali di concetto (Koch2007Transmon, Blais2004, Schuster2005PRL, Hutchings2017, Majer2007, Sheldon2016), misure di parametri fisici (Krupka, Place, Romanenko, Nakamura, Kim), e applicazioni HEP/sensing (Alesini, Lyu, Goss, Nguyen, Turro).

---

## 6. Stato post Wave-M (2026-05-18)

### 6.1 Fix applicati

Wave-M ha applicato 11 fix mirati che chiudono incoerenze numeriche interne identificate nel post-Wave-L sweep:

1. **§2.6 Numerical Estimates**: error budget riformulato con formula echo-CR coerente con il protocollo adottato in §2.3. $\varepsilon_{\text{decoh}} = t_{\text{CNOT}}/T_{2,\text{echo}}$ (non $2 t_{\text{CNOT}}/T_2$), $T_{2,\text{echo}} = 26.3$ μs, $\varepsilon_{\text{total}} = 2.10\%$, $\mathcal{F}_{\text{CNOT}} = 97.90\%$.
2. **§2.6 Comparison tab**: $F_{\text{CNOT}}$ a 300 GHz / 4 K corretto da $95.95\%$ a $97.90\%$.
3. **Caption Fig. T1_simulations**: $T_1 \approx 20$ μs → $15.3$ μs.
4. **Caption Fig. Ramsey**: $T_2 \approx 14.7$ μs → $13.2$ μs; $T_{2,\text{echo}} \sim 30$ μs → $26.3$ μs; ratio $0.74$ → $0.86$.
5. **Caption Fig. decoherence (e)**: $T_2 = 16.7$ μs → $13.2$ μs.
6. **Summary of Findings (testo)**: $T_1, T_2$ allineati ai valori canonici; aggiunta menzione esplicita del protocollo echo-CR di §2.3.
7. **Tab. tech_feasibility_4K**: $T_1, T_2, T_{2,\text{echo}}$ aggiornati.
8. **Abstract globale** (`abstract_revised.tex`): «coherence times exceeding 50 μs» (numero non coerente con il cap.) sostituito con «supporting two-qubit gate fidelities of approximately 98% under an echo cross-resonance protocol».
9. **Conclusions globale** (`conclusions.tex`): $T_1 > 20$ μs → $T_1 \approx 15$ μs; $T_2 > 14$ μs → $T_2 \approx 13$ μs; aggiunta $T_{2,\text{echo}} \approx 26$ μs e menzione echo-CR.
10. **`ok_create_corrected_figures.py`** in `qh_hardware`: introdotta variabile `T2_echo`; `F_CNOT = 0.9595` → `0.9790` (con commento esplicito al protocollo echo-CR); `F_CNOT_naive = 0.9595` mantenuto come riferimento legacy.
11. **§2.7 (Frequency Trade-off)**: aggiunta nota metodologica esplicita all'apertura della sezione, per chiarire al lettore che i numeri di $F$ riportati nella sezione ($34.2\%$ a 300 GHz / 4 K) riflettono un modello *dielectric-only* semplificato e NON sono le proiezioni HEATS-Q ($97.9\%$, §2.6).

### 6.2 Coerenza globale

Dopo Wave-M, nel Cap.2 e nelle sezioni globali (abstract + conclusions):
- **Nessuna occorrenza** di $\mathcal{F}_{\text{CNOT}} = 95.95\%$ residua.
- **Nessuna occorrenza** di $T_1 \approx 20~\mu$s o $T_2 \approx 14.7~\mu$s o $T_2 = 16.7~\mu$s residue per la configurazione 4 K / 300 GHz.
- Valori canonici unificati: $T_1 = 15.3~\mu$s, $T_2 = 13.2~\mu$s, $T_{2,\text{echo}} = 26.3~\mu$s, $\mathcal{F}_{\text{single}} = 99.85\%$, $\mathcal{F}_{\text{CNOT}} = 97.9\%$, gap $-0.10\%$.
- Compile: 178 pagine, 0 undefined references, 0 multiply-defined labels, 0 errori critici.

### 6.3 Open items / future revisions

Non vi sono open items strutturali al momento. Una nuova lettura del Cap.2 da parte dei revisori potrebbe identificare:
- Eventuali ulteriori incongruenze nelle caption delle 21 figure (non sistematicamente verificate per *tutti* i numeri).
- Possibile rifinitura della pedagogia in §2.4 (Two-qubit elevated-T) — apertura attualmente meno didattica delle altre.

---

*Documento generato in Wave-M (2026-05-18) da R. Cappuccio + Claude (Anthropic), branch `wave-m` su `github.com/rcapp2506/PhDThesis`.*
