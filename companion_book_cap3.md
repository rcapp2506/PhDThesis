# Companion book — Capitolo 3 «Quantum Algorithms and Hybrid Variational Architectures»

Documento di accompagnamento al Cap. 3 della tesi di dottorato di Roberto Cappuccio, *New Perspectives on Quantum Technologies* (Università di Siena, Ciclo XXXVII). Versione post Wave‑K + revisione Roberto del 17 maggio 2026 (HEAD `d3b1025`) + Wave‑L del 18 maggio 2026 con aggiunta del **noise‑resilience study di Sec. 3.4.10** (HEAD `cd9a058`).

Scopo: fornire, in formato compatto, (a) la **mappa narrativa** del capitolo, (b) il **filo logico** che lega le sezioni, (c) la **rassegna sintetica delle 41 referenze attive nel capitolo** con sintesi del contenuto e motivazione dell'inserimento, e (d) la **separazione esplicita** fra ciò che è adattato da Filippi e ciò che è contributo originale di Cappuccio.

---

## 1. Mappa narrativa del capitolo

| § | Titolo | Cosa contiene | Originale / Adattato |
|---|---|---|---|
| 3.1 | Introduction | Inquadramento NISQ, anticipazione contributi (multi‑seed stats, runtime model, hardware transfer) e dichiarazione esplicita di cosa è preso da Filippi | Originale (testo); cita Filippi per ansatz |
| 3.2 | Background | QML, encoding, parameter‑shift, barren plateaus, locality argument per Q‑CNN | Originale di sintesi su letteratura |
| 3.2.1 | Classical CNNs | Conv2d, pooling, cross‑entropy. Frase di transizione che esplicita la scelta del matched‑capacity classical control (≠ LeNet di Filippi) | Originale |
| 3.2.2 | Quantum ML and QCNNs | Modello PQC, encoding, MERA‑like hierarchy | Originale di sintesi |
| 3.3 | Methods | Quantum convolutional layer, architectures, dataset, training, runtime model | Misto |
| 3.3.1 | Quantum Convolutional Layer | Definizione formale Eq. (3.3)–(3.5), Fig. 3.2 da Schuld‑Petruccione | Eq. originali, Fig. 3.2 adapted |
| 3.3.2 | QCNN Architectures | Spazio delle varianti C16‑Q64, Q‑C6, etc., Fig. 3.3/3.4 da Filippi | Testo originale, fig. adapted |
| 3.3.3 | Dataset and Preprocessing | Sottoinsieme binario EuroSAT | Originale |
| 3.3.4 | Training and Optimization | Adam, parameter‑shift, ottimizzazione two‑phase sim→HW | Originale |
| 3.3.5 | Runtime‑aware metrics | Modello a priori del costo per epoca su QPU credit‑bounded, Alg. 3.1 | **Contributo originale Cappuccio** |
| 3.4 | Results | | |
| 3.4.1 | Dataset description | EuroSAT esempi, Fig. 3.5 | Adapted |
| 3.4.2 | Architectural search: the C16–Q64 winner | Single‑paragraph attribution of the architecture search to Filippi's M.Sc. thesis (Pisa, co‑supervised by R. Cappuccio and O. Morsch), Tab. 3.1 with `tab:qcnn-variants` reproduced from Filippi | Riferimento esplicito a Filippi come tesi di laurea magistrale di Pisa che ho co‑relazionato |
| 3.4.3 | Comparison to baselines | Filippi reference + nostre originali contribuzioni (matched/high‑capacity controls, multi‑seed, HW) | Originale + cita Filippi |
| 3.4.4 | Statistical uncertainty across runs | Protocollo R=10, train/val 80/20, paired Wilcoxon, McNemar | **Contributo originale Cappuccio** |
| 3.4.5 | Results and Statistical Robustness | **3‑level evidence ladder** (CCNN‑big, CCNN‑small, QCNN sim) + Level 4 Emerald, Fig. 3.7–3.13, paired Wilcoxon esatto two‑sided su 3 confronti | **Contributo originale Cappuccio Wave‑K** |
| 3.4.6 | Training Dynamics | Plateau ~0.945/0.970, sorpasso epoca 3 | Originale |
| 3.4.7 | Reproducibility and controls | Pooling ablation | Originale |
| 3.4.8 | Runtime on simulator vs hardware | Bridge a §3.4.9 | Originale |
| 3.4.9 | Hardware validation on Emerald | Backend setup, transpilation, cost model, mini‑batch SGD, hw characterization, fine‑tune, Fig. 3.16–3.18, Tab. 3.4–3.6 | **Contributo originale Cappuccio Phase D+E** |
| 3.4.10 | **Noise resilience under calibrated IBM Heron r2 noise model** | Paired multi‑seed (R=10) study on 4‑qubit reduction: noiseless vs noisy (ibm_fez restricted to 4 qubits). 6 subsubsection: experimental design, final classification accuracy ($p=0.062$), transient perturbation early epochs ($p<10^{-4}$, $d_z=-1.80$), convergence metrics, interpretation, limitations. Fig. 3.19–3.20, Tab. 3.7 | **Contributo originale Cappuccio Wave‑L (post‑Wave‑K complementary noise study)** |
| 3.4.11 | Takeaways | Late fusion stable, HW transfer, **noise‑resilience consistent with classical head absorbing additive perturbation** | Originale |
| 3.5 | Discussion | Statistical robustness, training‑efficiency vs asymptotic, **noise resilience in studied regime**, limitations | Originale |
| 3.6 | Conclusions | Late fusion, noise‑resilient QML vs phase‑fragile coherent algorithms | Originale |

In sintesi: il capitolo è **costruito sull'ansatz di Filippi**, ma il **80 %+ del testo, tutte le statistiche multi‑seed, l'intera Sec. 3.4.9 di hardware validation, il cost model di Sec. 3.3.5 e l'intera Sec. 3.4.10 di noise resilience sotto NoiseModel calibrato sono contributi originali di questa tesi**. Filippi è citato esattamente sette volte (la 8ª è già un'incidentale `Filippi encoding`), in due forme distinte: (i) tre `Adapted from~\cite{Filippi2025Thesis}` nelle caption delle figure derivate (Fig. 3.1, Fig. 3.3, Fig. 3.4); (ii) quattro citazioni inline che inquadrano i punti di partenza scientifici nel paragrafo introduttivo, in sec:classical-cnns, nel preambolo di sec:results, e in sec:baselines.

---

## 2. Filo logico sezione‑per‑sezione

La narrazione segue una progressione precisa che vale la pena rendere esplicita perché non sempre l'ordine dei subsection lo rende immediato.

L'apertura del capitolo dichiara, prima di ogni dettaglio tecnico, il punto di partenza (Filippi) e i **quattro** vettori di originalità (stats, runtime, hardware, noise resilience). Il **Background** introduce il vocabolario quantum‑ML (encoding, ansatz, parameter‑shift, barren plateaus) e culmina con un paragrafo «Implications for hybrid Q‑CNNs» che giustifica le quattro scelte di design (encoding rotazionale per patch, entangling shallow, misurazioni come feature, training via parameter‑shift). Da lì il **Methods** sviluppa formalmente ognuna delle quattro: la struttura del quantum convolutional layer, lo spazio delle architetture, il dataset binario EuroSAT, e l'ottimizzazione two‑phase (sim‑first poi HW). La novità metodologica forte del Methods è il **runtime‑aware cost model** di §3.3.5: una catena di equazioni esplicite che lega $K$ (parallel blocks), $n_w$ (trainable quantum weights), $m$ (mini‑batch), $p_{\text{img}}$ (patches per image) e $\tau_{\text{circ}}^{\text{wall}}$ al credit‑cost per epoca, calibrato a posteriori contro la billing IQM.

I **Results** procedono dal sim al sim‑avanzato all'hardware. §3.4.2 mostra le **curve di pre‑training** della singola‑seed e introduce il termine «pre‑training» con il razionale ora esplicito: il checkpoint di fine training simulatore è quello caricato su Emerald per il fine‑tuning hardware (§3.4.9), e questo split sim+HW è imposto dall'economia del budget credit. §3.4.3 esplora lo spazio delle architetture (Late vs Early Fusion) e identifica `C16‑Q64` come candidato. §3.4.4 lo confronta con due baseline: la pubblicazione di Sebastianelli (qualitativa, no seed) e il lavoro precedente di Filippi (LeNet baseline). §3.4.4 è il punto in cui dichiariamo che la LeNet di Filippi non è matched‑capacity, e che la nostra estensione introduce **due controlli classici** — Level 1 CCNN‑big (high‑capacity, ~85.6 M params) e Level 2 CCNN‑small (matched, 463 898 params). §3.4.5 enuncia il protocollo statistico (R=10 seed, split fissato, paired tests). §3.4.6 — il **cuore della Wave‑K** — è dove i tre simulator levels vengono confrontati con paired Wilcoxon esatto two‑sided su 10 seed: 1 vs 2 (capacity effect, $p=0.027$), 3 vs 2 (pere con pere, $p=0.027$), 3 vs 1 (asymmetric, $p=0.004$). Il messaggio Watson‑&‑Crick: training‑efficiency favours quantum, asymptotic accuracy favours classical, both readings significant al 5 %.

§3.4.9 chiude il loop sull'hardware reale. Quattro subsubsection raccontano in ordine: il setup del backend e la sanity check Bell, la transpilazione su topologia partially disjoint con bug di Qiskit fino a $K_{\max}=11$, il **cost model calibrato** contro la billing (Eq. 3.36, $\lceil\text{runtime}_s\rceil \times 0.75$ verificato su 184 jobs), e la **fine‑tuning campaign** di due epoche su Emerald che recupera il 100 % di validation accuracy dal checkpoint sim. La sezione si chiude con cinque «implications for QPU‑bound training» che costituiscono il knowledge‑transfer del capitolo: topology‑aware design non è opzionale, il limite di parallel blocks è una proprietà del toolchain non del device, i cost model vanno calibrati non assunti nominali, la calibration‑aware layout aiuta ma non elimina i per‑block bias, e parameter‑shift fine‑tuning su HW noisy converge nel piccolo regime di parametri.

§3.4.10 (Wave‑L, post‑Wave‑K) **estende la noise validation oltre il budget cloud‑credit** che limita la §3.4.9 a due sole epoche su device. Il design è di accoppiamento esatto: si prende un riduzione 4‑qubit del ParallelQuanv ansatz (un solo blocco $K=4$ sub‑circuiti, $n=4$ qubits ciascuno, kernel $2\times 2$, stride 3, 500 shots), si fa girare $R=10$ seed identici (range $[42,51]$) in due rami che differiscono **solo** nel backend del sampler: \texttt{statevector} (noiseless) vs \texttt{density\_matrix + ibm\_fez NoiseModel} ristretto ai 4 qubit usati. Il NoiseModel include gate errors $\bar{\varepsilon}_\text{sx}=0.90\%$, $\bar{\varepsilon}_\text{cz}=2.80\%$, readout errors, $T_1/T_2$ relaxation; con 17 sx + 6 cz dopo transpilation, fidelity cumulativa $F_{4q}\approx 0.73$ per pass. Il risultato è doppio: **accuracy finale statisticamente indistinguibile** ($96.40\pm 1.47\%$ vs $96.20\pm 1.36\%$, paired Wilcoxon $p=0.062$) e **transient perturbation early‑epoch significativa** (epoche 1–3, $\Delta_{\text{val\_loss}}=-1.14\pm 0.64 \times 10^{-2}$, paired Wilcoxon $p<10^{-4}$, Cohen's $d_z=-1.80$, very large effect), completamente assorbita da epoca 5 ($p=0.23$). Tutte le 5 metriche di convergenza paired non sono significative al $\alpha=0.05$.

L'interpretazione che il testo dà è **doppia e cauta**: (a) la $\sim 27\%$ infidelity cumulativa per‑pass del quantum layer produce una perturbazione additiva sul gradient che la classical head ($\sim 10^4$ trainable parameters) assorbe perché l'errore è uncorrelated tra patches e channels (sub‑circuiti indipendenti, shot draws indipendenti); (b) il quadro contrasta esplicitamente con la regime algoritmica (QPE, Hamiltonian simulation, HHL, citando NielsenChuang2011) dove l'errore è phase‑coherent e si accumula linearmente in depth — suggerendo che variational QML del nostro tipo possa essere un target naturale per NISQ, **nel regime studiato**. Tre limitations chiudono la sezione: task binario, ansatz shallow ($23$ gate per sub‑circuit), e NoiseModel snapshot‑based (no coherent crosstalk modes).

§3.5 (Takeaways), §3.6 (Discussion) e §3.7 (Conclusions) ricapitolano il messaggio scientifico e dichiarano i limiti: il binario EuroSAT è saturo, abbiamo un solo device, e la classical baseline che batte il QCNN al plateau non è stata indipendentemente tunata per quel task — non claim‑iamo nulla di più.

---

## 3. Rassegna delle 41 referenze attive

Per ogni riferimento riporto: **anno**, **autore principale**, **sintesi del contenuto in una riga**, **motivazione dell'inserimento nel cap. 3** (perché ci serve), **numero di occorrenze nel cap. 3**.

### Letteratura QML fondazionale

| Bibkey | Anno | Autore | Contenuto | Motivazione cap. 3 | # |
|---|---|---|---|---|---|
| `SchuldPetruccione2021` | 2021 | Schuld & Petruccione | Monografia di riferimento su Machine Learning con Quantum Computers; framework concettuale per encoding, ansatz, training | Pilastro teorico del Background; Fig. 3.2 (qconv schematic) è adapted da Fig. 5.16 del libro | 8 |
| `Schuld2019PRA` | 2019 | Schuld & Killoran | Supervised learning come feature‑map in spazi di Hilbert; introduce la prospettiva «quantum kernel» | Giustifica l'interpretazione del nostro encoding $S(\mathbf p)$ come feature map | 7 |
| `Havlicek2019` | 2019 | Havlíček, Córcoles et al. (Nature) | Supervised learning con quantum‑enhanced feature spaces; introduce gli encoder IQP‑style | Citato come alternativa di encoding (kernel/phase feature maps) non adottata da noi, e come feature‑map perspective | 8 |
| `Benedetti2019` | 2019 | Benedetti et al. | Review su Parameterized Quantum Circuits come ML models | Pilastro per la nozione di PQC come hypothesis class; introduce il decoupling $S(\mathbf p) \cdot W(\boldsymbol\theta)$ | 4 |
| `cerezo2021` | 2021 | Cerezo et al. (Nature Rev. Phys.) | Review «Variational quantum algorithms»; ansatz, training, applications, trainability | Citata 10× come riferimento generale su VQA, hardware‑efficient ansatz, e barren‑plateau theory | 10 |
| `biamonte2017` | 2017 | Biamonte et al. (Nature) | Survey storica «Quantum machine learning» | Citata una sola volta nell'intro come riferimento all'origine del campo | 1 |
| `Preskill2018` | 2018 | Preskill | Definizione del termine «NISQ era» | Apre l'introduzione del capitolo: «Quantum computing has entered the NISQ era» | 1 |

### Encoding e re‑uploading

| Bibkey | Anno | Autore | Contenuto | Motivazione cap. 3 | # |
|---|---|---|---|---|---|
| `PerezSalinas2020` | 2020 | Pérez‑Salinas et al. (Quantum) | Data re‑uploading per quantum classifier universale; mostra che ri‑iniettare i dati ad ogni layer aumenta l'espressività | Riferimento per Eq. (3.4) di re‑uploading; menzionato 7× nelle discussioni di expressivity | 7 |
| `Schuld2020Expressivity` | 2021 | Schuld, Sweke, Meyer (PRA) | Effetto del data encoding sull'expressive power; spettro di Fourier accessibile cresce con depth e re‑uploading | Co‑citato sistematicamente con `PerezSalinas2020` per l'argomento spettrale | 7 |
| `Schuld2019Grad` | 2019 | Schuld, Bergholm et al. (PRA) | Parameter‑shift rule per analytic gradients su hardware | Eq. (3.6) e Eq. (3.34): la regola di derivazione che usiamo per ogni gate generato da Pauli | 3 |
| `Mitarai2018` | 2018 | Mitarai et al. (PRA) | Quantum circuit learning; introduce indipendentemente il parameter‑shift | Co‑citato con `Schuld2019Grad` come fonte originale del parameter‑shift | 3 |

### QCNN e MERA hierarchy

| Bibkey | Anno | Autore | Contenuto | Motivazione cap. 3 | # |
|---|---|---|---|---|---|
| `Cong2019` | 2019 | Cong, Choi, Lukin (Nat. Phys.) | Quantum convolutional neural networks: architettura QCNN canonica con conv‑pool ricorsivo MERA‑like | Citata 9× come riferimento foundational per la QCNN; Eq. (3.2) e (3.7) di pooling | 9 |
| `Vidal2007MERA` | 2007 | Vidal (PRL) | Entanglement renormalization (MERA); albero gerarchico di disentangling | Citato una volta come riferimento storico per la struttura gerarchica MERA‑like della QCNN | 1 |
| `Pesah2021PRX` | 2021 | Pesah, Cerezo et al. (PRX) | Absence of barren plateaus in QCNN; le QCNN evitano la concentrazione esponenziale | Citato 5× ogni volta che giustifichiamo la nostra scelta di una struttura locality‑preserving | 5 |

### Trainability, barren plateaus, noise

| Bibkey | Anno | Autore | Contenuto | Motivazione cap. 3 | # |
|---|---|---|---|---|---|
| `McClean2018` | 2018 | McClean et al. (Nat. Commun.) | Articolo originale sui barren plateaus in quantum neural networks | Citato 5× come riferimento del problema | 5 |
| `Holmes2022PRXQ` | 2022 | Holmes et al. (PRX Quantum) | Connessione fra expressibility dell'ansatz e magnitude dei gradienti | Citato 7× per giustificare le scelte di entanglement range moderato | 7 |
| `Marrero2021PRXQ` | 2021 | Ortiz Marrero et al. (PRX Quantum) | Entanglement‑induced barren plateaus | Citato 3× nelle discussioni di expressivity vs trainability | 3 |
| `Wang2021NoiseInducedBP` | 2021 | Wang, Cerezo et al. | Noise‑induced barren plateaus in VQA | Citato 2× per i limiti di scaling del HW fine‑tuning | 2 |
| `Caro2022NatCommun` | 2022 | Caro et al. (Nat. Commun.) | Generalization bounds in quantum ML da pochi dati | Citato 2× per il sample‑efficiency argument nella Discussion | 2 |
| `Kandala2017` | 2017 | Kandala et al. (Nature) | Hardware‑efficient VQE; introduce gli ansatz «hardware‑efficient» | Citato una volta per la nostra scelta di un hardware‑efficient ansatz | 1 |

### Lavoro precedente da cui prendiamo l'ansatz e l'architettura

| Bibkey | Anno | Autore | Contenuto | Motivazione cap. 3 | # |
|---|---|---|---|---|---|
| `Filippi2025Thesis` | 2025 | Daniele Filippi | Tesi di laurea magistrale in Fisica **all'Università di Pisa, sotto la co‑relazione di R. Cappuccio e Oliver Morsch**: hybrid quantum‑classical CNN per dati remote sensing, ansatz parallel block, single‑seed comparison vs LeNet‑5, ablation della Sec. 3.4.3 | **Il lavoro di tesi di laurea magistrale che ho co‑relazionato (Pisa) e su cui poggiamo l'ansatz**. Tre figure (3.1, 3.3, 3.4) sono «Adapted from» Filippi; la prima citazione nell'intro ha footnote «*M.Sc. thesis in Physics at the University of Pisa, co‑supervised by the present author together with Oliver Morsch*»; la Sec. 3.4.3 dichiara esplicitamente che l'architectural search «*was performed in the prior master's thesis of Filippi, carried out at the University of Pisa under the co‑supervision of the present author and Oliver Morsch*» | 11 |
| `Sebastianelli2021` | 2021 | Sebastianelli et al. | Hybrid quantum‑inspired e quantum ML per remote sensing | Citato come reference qualitativa nei baseline (single‑seed, no paired stats possibile) | 6 |
| `LeCun1998` | 1998 | LeCun, Bottou, Bengio, Haffner | Gradient‑based learning, LeNet‑5 | Baseline classica storica usata da Filippi; noi la sostituiamo con matched‑capacity controls | 6 |
| `Goodfellow2016` | 2016 | Goodfellow, Bengio, Courville | Manuale «Deep Learning» | Pilastro per la sezione di CNN classiche (Conv2d, BatchNorm, Dropout, backprop) | 5 |
| `Helber2019` | 2019 | Helber et al. (IEEE JSTARS) | EuroSAT: dataset di 27 000 patch Sentinel‑2, 10 classi land‑use | Il dataset da cui estraiamo il sottoinsieme binario Forest vs AnnualCrop | 5 |

### Statistica

| Bibkey | Anno | Autore | Contenuto | Motivazione cap. 3 | # |
|---|---|---|---|---|---|
| `Wilcoxon1945` | 1945 | Wilcoxon | Articolo originale del paired signed‑rank test | Test paired esatto two‑sided su 3 confronti in sec:results-stats | 2 |

### Qiskit, BackendV2, Target, OpenQASM 3 (toolchain)

| Bibkey | Anno | Autore | Contenuto | Motivazione cap. 3 | # |
|---|---|---|---|---|---|
| `Qiskit2019` | 2019 | Qiskit Contributors | Citation ufficiale del progetto Qiskit | Citato 3× come framework | 3 |
| `QiskitBackendV2Docs` | 2025 | IBM Quantum | Documentation `BackendV2` (API moderna) | Citato 4× per i metodi di timing e calibration | 4 |
| `QiskitTargetDocs` | 2025 | IBM Quantum | Documentation `Target` (transpiler) | Co‑citato con BackendV2 per la pipeline di scheduling | 4 |
| `QiskitEstimatorDocs` | 2025 | Qiskit Community | Documentation della Primitives `Estimator` | Citato 3× per la pipeline di expectation values | 3 |
| `OpenQASM3TQC2022` | 2022 | Cross, Javadi‑Abhari et al. (TQC) | Specifica OpenQASM 3 | Citato 3× per la semantica di timing | 3 |
| `McKay2018BackendSpec` | 2018 | McKay et al. | Backend Specifications per OpenQASM e OpenPulse | Citato una volta per il contesto storico (legacy BackendV1 → Qobj → OpenPulse) | 1 |
| `InstructionDurations` | 2025 | Qiskit Community | Doc API di `InstructionDurations` | Citato una volta per ASAP/ALAP scheduling | 1 |
| `ASAPScheduleAnalysis` | 2025 | Qiskit Community | Doc del pass `ASAPScheduleAnalysis` | Citato una volta per lo scheduling | 1 |
| `Li2019Sabre` | 2019 | Li, Ding, Xie | Tackling the qubit mapping problem in NISQ devices, Sabre layout pass | Citato una volta nel contesto della transpilation Sec. 3.4.9 | 1 |

### IQM hardware (Emerald + Resonance)

| Bibkey | Anno | Autore | Contenuto | Motivazione cap. 3 | # |
|---|---|---|---|---|---|
| `IQMClientDocs2025` | 2025 | IQM Quantum Computers | `iqm-client` Qiskit adapter user guide | Citato 2× per il software stack del HW campaign | 2 |
| `QiskitOnIQMGuide` | 2025 | IQM Quantum Computers | User guide Qiskit on IQM | Co‑citato con `IQMClientDocs2025` | 2 |
| `IQMResonance2025` | 2026 | IQM Quantum Computers | IQM Resonance: cloud access to IQM quantum computers | Citato una volta nell'introduzione di Sec. 3.4.9 | 1 |

### CAD / latency estimation per quantum circuits

| Bibkey | Anno | Autore | Contenuto | Motivazione cap. 3 | # |
|---|---|---|---|---|---|
| `Dousti2013LEQA` | 2013 | Dousti, Pedram (DAC) | LEQA: latency estimation per quantum circuits | Citato 2× come precedente CAD del nostro cost model di Sec. 3.3.5 | 2 |
| `Lao2020TimingMapping` | 2020 | Lao, Wille et al. | Timing‑ e resource‑aware mapping per NISQ | Co‑citato con Dousti per il riferimento al timing‑aware approach | 2 |
| `GateAwareDepth2025` | 2025 | Tremba, Hovland, Liu | «Is circuit depth accurate for comparing runtimes?» — gate‑aware depth | Citato una volta nella Discussion per la critica dello plain depth | 1 |

---

## 4. Il confine fra Filippi e contributo originale

Per chiarezza nei tre puntelli su cui poggia l'argomento:

**Adapted from Filippi** (con esplicita attribuzione come tesi di laurea magistrale di Pisa, co‑relazionata da R. Cappuccio e O. Morsch, nella footnote alla prima citazione del capitolo):
- Figura 3.1 `fig:hybrid_overview` (schema generale di hybrid pipeline) — adapted from [Filippi2025Thesis]
- Figura 3.3 `fig:qconv` (TwoLocal ansatz, 9 qubit con alternati $R_Y/R_Z$ + CNOT) — adapted from [Filippi2025Thesis]
- Figura 3.4 `fig:qcnn_ansatz` (variational circuit fully connected analog) — adapted from [Filippi2025Thesis]
- Eq. (3.21) `eq:parallel_circuit` del parallel‑block transpilation circuit, che era proposta in Filippi per K=1 e che noi estendiamo a $K=11$ blocks
- L'architettura `C16‑Q64` come architecture‑search winner di Sec. 3.4.3 e Tab. 3.1 — reproduced from Filippi
- La frase di sec:results-curves che dice «10 epochs are sufficient to bring both architectures past the plateau documented in `Filippi2025Thesis`»
- L'**intera Sec. 3.4.3 «Architectural search: the C16–Q64 winner»**, ora un singolo paragrafo che dichiara esplicitamente: *«was performed in the prior master's thesis of Filippi, carried out at the University of Pisa under the co-supervision of the present author and Oliver Morsch»*

**Originale Cappuccio**:
- L'intero protocollo statistico R=10 multi‑seed di sec:stats-significance — Filippi era single‑seed
- L'intera tabella di paired Wilcoxon esatto two‑sided su 3 confronti (Level 1‑2, Level 2‑3, Level 1‑3) — non esiste in Filippi
- I controlli matched‑capacity CCNN‑small (463 898 params) e high‑capacity CCNN‑big (~85.6 M params) — Filippi usa LeNet‑5
- Il cost model di Sec. 3.3.5 (Eqs. 3.10‑3.14) — non esiste in Filippi
- L'intera Sec. 3.4.9 di hardware validation su Emerald (backend setup, transpilation analysis, $K_{\max}=11$ identification, calibration‑aware initial layout, cost model calibration, mini‑batch SGD adaptation, fine‑tune results) — non esiste in Filippi
- L'intera Sec. 3.4.10 di noise resilience sotto NoiseModel ibm_fez calibrato (R=10 paired seeds, statevector vs density_matrix branch, paired Wilcoxon test, transient perturbation analysis su epochs 1–3, table di metriche di convergenza) — non esiste in Filippi
- Il bridge to hardware (Level 4) con paired Wilson 95 % CI — non esiste in Filippi
- Tutte le item‑level McNemar contingency tables — non esistono in Filippi

Il «Our contributions» dell'introduzione, ora riformulato, esplicita tutto questo prima del lettore.

---

## 5. Note tecniche di compilazione

- PDF: 175 pagine totali, 48 pagine Cap. 3 (pp. 81‑128), 0 undefined refs, 0 undefined citations, 0 multiply‑defined labels.
- Tool versione: TeX Live 2023 (Ubuntu Noble), `lmodern` package, `pdflatex 1.40.25`, `bibtex 0.99d`.
- Bib file: `chapters/full_references_checked.bib`, 145 entries totali, 41 univoche attive nel Cap. 3 (verifica con regex Python su testo non‑commentato).
- Compile cycle: `pdflatex → bibtex → pdflatex → pdflatex`.
- Stato repo: HEAD `wave-K-cap3` su `github.com/rcapp2506/PhDThesis`, ultimo commit ≥ `117e724` (fix `tab:arch_search` → `tab:qcnn-variants`).

---

## 6. Open items dopo questa lettura

1. **Punto 3.5 noisy** (`ibm_fez`): non in questo commit. Aspetta lo smoke su Magellano. Quando torna, integriamo in sec:results-stats come Level 3.5.
2. **Item‑level McNemar Level 3 vs Level 2**: serve allineamento per‑image dei due dataset di validation (clerical, non statistico). Non blocca la difesa.
3. **Caveat asymmetry of readout error**: in Tab. `tab:per_block_bias` discusso brevemente; un'analisi più approfondita potrebbe entrare in un follow‑up paper.

— *Documento aggiornato 17 maggio 2026, post‑Wave‑K consolidation.*
