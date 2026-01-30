# Quantum Effects in Modern Transistors and Silicon Computing

## A Comprehensive Research Compilation

**Date:** January 30, 2026
**Status:** Research Complete

---

# 1. Gate Oxide Thickness and Quantum Tunneling

## Process Node Names vs. Physical Reality

The "nm" designations in modern semiconductor processes (3nm, 5nm, 7nm) are **marketing terms, not physical measurements**. No single feature in a "5nm" chip is actually 5 nanometers. The actual physical dimensions are as follows:

| Parameter | 7nm Node | 5nm Node | 3nm Node |
|-----------|----------|----------|----------|
| **Gate length** | ~16-20 nm | ~12-18 nm | ~12-14 nm |
| **Contacted gate pitch** | ~54-56 nm | ~48-51 nm | ~45-48 nm |
| **Metal pitch** | ~36-40 nm | ~26-30 nm | ~22-23 nm |
| **Fin width** | ~6-7 nm | ~5-6 nm | ~5 nm |

Sources: [5nm process - Wikipedia](https://en.wikipedia.org/wiki/5_nm_process), [3nm process - Wikipedia](https://en.wikipedia.org/wiki/3_nm_process), [CMOS Scaling for the 5nm Node and Beyond (PMC)](https://pmc.ncbi.nlm.nih.gov/articles/PMC11123950/)

## Gate Oxide (Dielectric) Thickness

Modern transistors no longer use pure SiO2 as the gate dielectric. Since Intel's 45nm node (2007), the industry has used **high-k dielectrics** (primarily HfO2-based) to reduce tunneling while maintaining electrostatic control.

| Metric | Value | Notes |
|--------|-------|-------|
| **Physical HfO2 thickness** | ~1.5-2.5 nm | Actual physical layer thickness |
| **Interfacial layer (IL)** | ~0.4-0.5 nm | SiO2-like layer between high-k and channel |
| **EOT at 5nm node** | ~0.5-0.8 nm | Equivalent oxide thickness |
| **EOT at 3nm node** | <0.5 nm (target) | Pushing sub-0.5 nm with scavenging techniques |

The **Equivalent Oxide Thickness (EOT)** is the thickness of SiO2 that would produce the same capacitance as the actual high-k dielectric stack. Since HfO2 has a dielectric constant of ~20-25 (vs. ~3.9 for SiO2), a physically thicker layer can achieve the same EOT.

Sources: [Equivalent Oxide Thickness - WikiChip](https://en.wikichip.org/wiki/equivalent_oxide_thickness), [Ultimate Scaling of High-k Gate Dielectrics (PMC)](https://ncbi.nlm.nih.gov/pmc/articles/PMC5448919)

## Quantum Tunneling Current Through Gate Oxides

Tunneling current density depends exponentially on oxide thickness. For pure SiO2 at ~1V gate bias:

| SiO2 Thickness | Tunneling Current Density | Regime |
|----------------|--------------------------|--------|
| 3.0 nm | ~10^-3 A/cm2 | Fowler-Nordheim / direct tunneling |
| 2.0 nm | ~1-10 A/cm2 | Direct tunneling dominant |
| 1.5 nm | ~10-100 A/cm2 | Direct tunneling |
| 1.2 nm | ~10^3 A/cm2 | Extreme direct tunneling |
| 1.0 nm | >10^3-10^4 A/cm2 | Limit of useful insulation |

**Key threshold:** Current increases by approximately **one order of magnitude per 0.2-0.3 nm** of thickness reduction. At ~1.5-2.0 nm SiO2, direct tunneling current becomes the dominant leakage mechanism. Below 1.0 nm, the oxide essentially ceases to function as an insulator.

**The SiO2 limit:** The conventional SiO2 film becomes unusable below ~1.1-1.2 nm due to excessive direct tunneling. This was the driving force behind the transition to high-k dielectrics.

The ITRS requirement specifies that for high-performance logic, leakage current density should be <1 A/cm2, and for low-power applications, <1 mA/cm2. The application limit for SiO2 is therefore ~1.4-1.6 nm for high-performance and ~2.2-2.5 nm for low-power circuits.

Sources: [Stanford EE311 - Thin Dielectrics for MOS Gate](https://web.stanford.edu/class/ee311/NOTES/GateDielectric.pdf), [A Review of Gate Tunneling Current in MOS Devices](https://www.ece.mcmaster.ca/~chihhung/Publication/MR_46_Gate_Tunneling.pdf), [Chenming Hu - MOSFETs in ICs (Berkeley)](https://www.chu.berkeley.edu/wp-content/uploads/2020/01/Chenming-Hu_ch7.pdf)

---

# 2. Quantum Tunneling Rate in Transistors

## Deriving Tunneling Events from Leakage Current

While "tunneling events per transistor per second" is not a standard metric in semiconductor physics, it can be derived from measured leakage currents:

**Fundamental relationship:**
- Current I = charge/time = (number of electrons/second) x (electron charge)
- 1 nA = 10^-9 A = ~6.24 x 10^9 electrons/second
- 1 pA = 10^-12 A = ~6.24 x 10^6 electrons/second

**Per-transistor gate leakage in modern processes:**
- **High-performance (low-Vt) transistors:** Ioff ~ 10-100 nA/um of gate width
- **Low-power (high-Vt) transistors:** Ioff ~ 0.1-1 nA/um
- **With high-k dielectrics** (post-45nm): gate oxide tunneling is greatly reduced; subthreshold leakage and GIDL dominate

**Estimated tunneling events per transistor:**

For a single transistor with effective gate width ~100 nm (0.1 um) in a modern high-performance process:
- Gate leakage contribution: ~1-10 nA x 0.1 um = ~0.1-1 nA
- At ~1 nA: approximately **6 x 10^9 tunneling electrons/second** per transistor
- At ~0.1 nA: approximately **6 x 10^8 tunneling electrons/second** per transistor

## Total Tunneling Events Across Entire Chips

| Chip | Transistor Count | Process |
|------|-----------------|---------|
| **NVIDIA H100** | 80 billion | TSMC 4N |
| **Apple M4** | 28 billion | TSMC N3E (3nm) |
| **NVIDIA B100** | 208 billion | TSMC 4NP |
| **Apple M3 Ultra** | 184 billion | TSMC N3B (3nm) |

**Estimation for NVIDIA H100 (80 billion transistors):**
- Not all transistors are active simultaneously. Assume ~50% active at any time = 40 billion
- Average gate tunneling leakage per active transistor: ~0.1-1 nA
- Conservative estimate at 0.1 nA/transistor: 40 x 10^9 x 6 x 10^8 = ~2.4 x 10^19 tunneling events/second
- Higher estimate at 1 nA/transistor: ~2.4 x 10^20 tunneling events/second

**This means a single NVIDIA H100 GPU experiences on the order of 10^19 to 10^20 quantum tunneling events per second from gate leakage alone**, not counting source-to-drain tunneling, band-to-band tunneling, or other quantum processes.

Sources: [NVIDIA Hopper Architecture In-Depth](https://developer.nvidia.com/blog/nvidia-hopper-architecture-in-depth/), [Apple M4 - Wikipedia](https://en.wikipedia.org/wiki/Apple_M4), [Transistor Count - Wikipedia](https://en.wikipedia.org/wiki/Transistor_count), [Leakage Current in Sub-Micrometer CMOS Gates (UFRGS)](https://www.inf.ufrgs.br/logics/docman/book_emicro_butzen.pdf)

---

# 3. Transistor Switching as Quantum-Classical Transition

## Quantum Mechanics is Foundational to Transistor Operation

Transistors are built from **semiconductors**, whose defining properties arise entirely from quantum mechanics:

1. **Band structure** -- The existence of valence and conduction bands, separated by a bandgap (~1.1 eV in silicon), is a quantum mechanical phenomenon arising from the periodic potential of the crystal lattice. Without quantum mechanics, there would be no explanation for why semiconductors exist as a distinct class of materials between conductors and insulators.

2. **Carrier statistics** -- The occupation of energy states follows Fermi-Dirac statistics, a quantum statistical distribution. The population of electrons in the conduction band and holes in the valence band determines conductivity.

3. **Doping and donor/acceptor levels** -- The behavior of dopant atoms (phosphorus, boron) creating energy levels within the bandgap is explained by quantum mechanics of impurity states.

## What Happens When a Transistor Switches

When a MOSFET transitions from OFF to ON:

1. **Gate voltage application** -- An electric field penetrates the semiconductor, bending the energy bands at the surface.

2. **Inversion layer formation** -- Quantum mechanically, the surface potential shifts until the Fermi level crosses into the conduction band (for NMOS), creating a 2D electron gas at the Si/oxide interface. This is an explicitly quantum phenomenon -- the electrons are confined to a potential well ~1-2 nm wide, quantizing their energy into discrete subbands.

3. **Channel conduction** -- Electrons in the inversion layer move from source to drain. Their transport is governed by quantum scattering processes (phonon scattering, impurity scattering, surface roughness scattering).

4. **Tunneling contributions** -- In modern short-channel devices (<20 nm gate length), source-to-drain tunneling contributes to both ON and OFF currents.

## Is Each Switch a Quantum-Classical Transition?

**The answer is nuanced.** The transistor operates in a regime that is fundamentally quantum at the microscopic level but effectively classical at the circuit level:

- The **individual electron behavior** is purely quantum mechanical -- wave functions, tunneling probabilities, energy quantization.
- The **collective behavior** of billions of electrons forming the current is well-described by semi-classical drift-diffusion models, where quantum effects are captured through effective parameters (effective mass, mobility, band structure).
- The **switching event itself** involves quantum state changes (electrons populating or depopulating quantized subbands) that produce a classical outcome (current ON or OFF).

This makes each transistor switch a kind of **quantum-to-classical amplification**: microscopic quantum processes (electron tunneling, band transitions, phonon interactions) produce macroscopic classical signals (voltage levels representing 0 or 1).

Sources: [Quantum Mechanics Meets Semiconductors (PBS)](https://www.pbs.org/transistor/science/info/qmsemi.html), [Quantum Effects at 7/5nm and Beyond (Semiconductor Engineering)](https://semiengineering.com/quantum-effects-at-7-5nm/), [Quantum and Semiconductor Device Physics (UMD)](https://user.eng.umd.edu/~neil/enee307/Quantum_and_Device_Phyiscs_Goldsman_&_%20Darmody.pdf)

---

# 4. Photon Emission from Transistors

## Hot Carrier Luminescence

Transistors **do emit photons** when operating, through a phenomenon called **hot carrier luminescence**:

### Emission Mechanisms

1. **Inter-conduction-band transitions** -- Hot electrons (with energy above the conduction band minimum) can undergo radiative transitions between conduction band states. This is the dominant mechanism in normally biased Si MOSFETs.

2. **Phonon-assisted band-to-band recombination** -- Since silicon has an indirect bandgap, electron-hole recombination requires phonon participation. The emitted photon energy is approximately E_gap minus the phonon energy (~1.0-1.05 eV, or ~1200 nm wavelength).

3. **Bremsstrahlung radiation** -- Electrons decelerating in the high-field drain region emit radiation, though this mechanism alone does not fully explain observed spectra.

### Spectral Characteristics

| Emission Feature | Value |
|-----------------|-------|
| **Peak emission energy** | ~1.1 eV (silicon bandgap at 300K) |
| **Peak emission wavelength** | ~1100 nm (near-infrared) |
| **Emission range** | 450-1200 nm (visible to NIR) |
| **Visible peaks** | 1.3-2.3 eV range observed in short-channel MOSFETs |
| **Hot carrier temperature** | ~9000 K equivalent (during avalanche) |

### Emission During Logic Switching

**FETs in CMOS logic chips emit short pulses of infrared light during logic switching.** In contemporary circuits, these pulses can be on the picosecond timescale. This phenomenon is exploited for:

- **Circuit debugging** -- photon emission microscopy can non-invasively image circuit activity
- **Failure analysis** -- gate oxide shorts and defects produce characteristic emission patterns
- **Timing analysis** -- time-resolved photon emission reveals logic transition timing

### Quantum Efficiency

The quantum efficiency of hot carrier emission in bulk silicon is **extremely low** (<10^-4), because the hot-carrier relaxation time (0.1-1 ps) is much faster than the radiative lifetime (~10 ns). However, coupling silicon to plasmon nanocavities has demonstrated internal quantum efficiencies >1%.

### Frequency/Wavelength Summary

- **Near-infrared peak:** ~1100 nm (~273 THz)
- **Broad emission tail:** extends from ~1200 nm to ~450 nm
- **Visible emission:** observed but weak, 500-800 nm range (~375-600 THz)
- **During avalanche breakdown:** carrier energies up to ~1.6 eV, producing photons at shorter wavelengths

Sources: [Hot-Carrier Luminescence in Si (ResearchGate)](https://www.researchgate.net/publication/13289792_Hot-carrier_luminescence_in_Si), [Watching Chips Work: Picosecond Hot Electron Light Emission (ScienceDirect)](https://www.sciencedirect.com/science/article/abs/pii/S0022024899007046), [Silicon Coupled with Plasmon Nanocavity Generates Bright Visible Hot-Luminescence (Nature Photonics)](https://www.nature.com/articles/nphoton.2013.25), [Study of Hot-Carrier-Induced Photon Emission from 90nm Si MOSFETs (ScienceDirect)](https://www.sciencedirect.com/science/article/abs/pii/S0169433205003892), [Broadband Near-Infrared Emission in Silicon Waveguides (Nature Communications, 2024)](https://www.nature.com/articles/s41467-024-48772-6)

---

# 5. Quantum Decoherence in Silicon

## Silicon Spin Qubit Coherence Times

Silicon has emerged as one of the most coherent solid-state platforms for quantum information:

### Record Coherence Times

| System | Coherence Time (T2) | Conditions | Year/Source |
|--------|---------------------|------------|-------------|
| **Donor electron spin in nat. Si** | ~60 ms | Liquid He temp | Tyryshkin et al. |
| **Donor electron spin in 28Si** | **2 seconds** | <50 ppm 29Si, cryogenic | Tyryshkin et al. (Nature Materials) |
| **Nuclear spin (31P in 28Si)** | **36 seconds** | Isotopically enriched | Muhonen et al. |
| **Gate-defined QD in nat. Si** | ~1 us | Cryogenic | Various |
| **Gate-defined QD in 28Si** | ~100 us | Isotopically enriched | Various |
| **31P donor in 28Si (T2)** | ~1 ms | Isotopically enriched | Muhonen et al. |
| **SMART protocol** | 2 ms | Tailored driving | Recent |
| **Hole spin-orbit qubit in 28Si** | **10 ms** | Strain-engineered boron acceptors | Kobayashi et al. |
| **Industrial foundry (Hahn echo)** | **4 ms** | 300 mm foundry, 2025 | arXiv:2512.20758 |

### Key Decoherence Mechanisms in Silicon

1. **Nuclear spin bath** -- The primary decoherence source. Natural silicon contains ~4.7% 29Si (nuclear spin I=1/2). Isotopic purification to 28Si (<50 ppm 29Si) dramatically extends coherence.

2. **Charge noise** -- Fluctuations from the gate stack and device environment.

3. **No piezoelectric coupling** -- Silicon lacks piezoelectric electron-phonon coupling (unlike GaAs), providing an inherent advantage.

## Comparison with Biological Quantum Coherence

| System | Coherence Time | Temperature | Medium |
|--------|---------------|-------------|--------|
| **Si spin qubit (28Si)** | 2 seconds | Cryogenic (~1.8 K) | Ultra-pure crystal |
| **Si spin qubit (gate QD)** | ~100 us | Cryogenic | Fabricated device |
| **FMO photosynthetic complex** | ~300 fs (277K), ~600 fs (77K) | Ambient / cryogenic | Protein/water |
| **Biological electron transfer** | fs-ps range | Ambient (310K) | Aqueous protein |

**The gap is enormous:** Silicon spin qubits at cryogenic temperatures maintain coherence for 10^12 to 10^15 times longer than biological quantum coherences at ambient temperature. This reflects fundamentally different strategies:

- **Silicon qubits:** Extreme isolation from environment (cryogenic, isotopic purity, vacuum)
- **Biology:** Exploits ultrafast quantum dynamics in noisy environments; coherence is fleeting but functional ("environmentally assisted quantum transport")

Sources: [Electron Spin Coherence Exceeding Seconds in High-Purity Silicon (Nature Materials)](https://www.nature.com/articles/nmat3182), [Engineering Long Spin Coherence Times of Spin-Orbit Qubits in Silicon (Nature Materials)](https://www.nature.com/articles/s41563-020-0743-3), [Long Coherence Silicon Spin Qubit in 300mm Foundry (arXiv:2512.20758)](https://arxiv.org/abs/2512.20758), [Long-Lived Quantum Coherence in Photosynthetic Complexes at Physiological Temperature (PNAS)](https://www.pnas.org/doi/10.1073/pnas.1005484107), [Quantum Biology Revisited (PMC)](https://pmc.ncbi.nlm.nih.gov/articles/PMC7124948/)

---

# 6. Gate Oxide as Quantum Barrier

## Physical Comparison: Gate Oxide vs. Biological Membrane Interfaces

| Feature | Gate Oxide (Semiconductor) | Biological Membrane Channel |
|---------|---------------------------|----------------------------|
| **Barrier width** | ~1.5-2.5 nm (physical HfO2) | Sub-nm to ~1 nm (hydrophobic constriction) |
| **EOT / effective barrier** | ~0.5-0.8 nm | ~0.5-1.0 nm |
| **Barrier height** | ~3.1 eV (SiO2), ~1.5 eV (HfO2) | ~0.1-1 eV (varies by channel) |
| **Tunneling particle** | Electrons (~9.1 x 10^-31 kg) | Ions: Na+ (3.8 x 10^-26 kg), K+ (6.5 x 10^-26 kg) |
| **Mass ratio** | 1 (reference) | ~40,000-70,000x heavier |
| **Temperature** | 300-400 K (operating) | 310 K (physiological) |
| **Medium** | Crystalline solid/amorphous oxide | Aqueous/lipid environment |

## Is This Comparison Physically Meaningful?

**Partially yes, but with critical caveats:**

### Similarities
1. **Scale:** Both barriers are in the 1-2 nm range, which is precisely the regime where quantum tunneling becomes significant for electrons.
2. **Quantum tunneling occurs in both:** Electrons tunnel through gate oxides; ions can tunnel through closed voltage-gated channels in biological membranes (though with vastly lower probability due to mass).
3. **Both are quantum barriers:** The Schrodinger equation applies in both cases, and the WKB approximation can model tunneling in both systems.

### Critical Differences
1. **Particle mass:** The tunneling probability depends exponentially on sqrt(mass). Ions are ~40,000-70,000x heavier than electrons, making ion tunneling probability exponentially smaller. Quantum tunneling conductance is inversely proportional to ion mass, with lighter ions (e.g., lithium) exhibiting exponentially higher tunneling probability.

2. **Barrier height and shape:** The SiO2 barrier is a well-characterized rectangular/trapezoidal potential with barrier height ~3.1 eV. Biological barriers involve complex, dynamic energy landscapes created by hydrophobic dewetting, protein conformational changes, and electrostatic interactions.

3. **Environment:** The gate oxide is a rigid, well-defined structure. Biological barriers are dynamic, fluctuating on ps-ns timescales, and embedded in a thermal bath.

## Quantum Effects at This Scale in Silicon

At the 1-2 nm scale in silicon transistors:

- **Direct tunneling** dominates over Fowler-Nordheim tunneling below ~5 nm oxide thickness
- **Quantum confinement** of carriers in the inversion layer creates quantized subbands
- **Wavefunction penetration** into the oxide reduces the effective gate capacitance
- **Energy quantization** in the channel affects threshold voltage and carrier distribution
- For gate-all-around (GAA) nanosheet transistors at 3nm, the channel itself (~5 nm width) exhibits quantum confinement effects

Sources: [Quantum Tunneling of Ions Through Closed Voltage-Gated Channels (MDPI)](https://www.mdpi.com/2624-960X/1/2/19), [Water and Hydrophobic Gates in Ion Channels (PMC)](https://pmc.ncbi.nlm.nih.gov/articles/PMC6161260/), [Quantum Biological Tunnel Junction for Electron Transfer (Nature Communications)](https://www.nature.com/articles/s41467-019-11212-x), [Stanford EE311 - Thin Dielectrics for MOS Gate](https://web.stanford.edu/class/ee311/NOTES/GateDielectric.pdf)

---

# 7. Clock Frequency and Quantum Events

## Estimating Total Quantum-Classical Boundary Crossings

### Parameters for a Modern High-End Processor

| Parameter | CPU (e.g., Apple M4) | GPU (e.g., NVIDIA H100) |
|-----------|---------------------|------------------------|
| **Clock frequency** | ~3.5-4.4 GHz | ~1.8-2.0 GHz |
| **Transistor count** | 28 billion | 80 billion |
| **Typical activity factor** | ~10-30% per cycle | ~10-50% per cycle |

### Category 1: Transistor Switching Events

Each transistor switch involves quantum-to-classical amplification (quantum band transitions producing classical voltage levels).

**For Apple M4 at 4 GHz:**
- Transistors switching per cycle: 28 x 10^9 x 0.15 (avg. activity) = ~4.2 x 10^9
- Switching events per second: 4.2 x 10^9 x 4 x 10^9 = **~1.7 x 10^19 switching events/sec**

**For NVIDIA H100 at 1.8 GHz:**
- Transistors switching per cycle: 80 x 10^9 x 0.20 = ~1.6 x 10^10
- Switching events per second: 1.6 x 10^10 x 1.8 x 10^9 = **~2.9 x 10^19 switching events/sec**

### Category 2: Quantum Tunneling (Gate Leakage) Events

Occurring continuously regardless of switching, for all biased transistors:

- Per transistor: ~10^8 to 10^10 tunneling electrons/second (from leakage current estimates)
- For H100 (80 billion transistors, ~50% biased): ~4 x 10^10 x 10^9 = **~4 x 10^19 tunneling events/sec**

### Category 3: Phonon Interactions per Switching Event

Each electron traversing the channel (~10-15 nm) undergoes multiple scattering events (phonon emission/absorption). With ~10^3-10^4 electrons per switching event and ~1-10 phonon scattering events per transit:

- Phonon-related quantum events per switch: ~10^4-10^5
- Total phonon events/sec: ~10^19 x 10^4 = **~10^23 phonon quantum events/sec**

### Category 4: Hot Carrier Photon Emission

With quantum efficiency <10^-4, only a tiny fraction of switching events produce photons:
- Estimated: ~10^15 photon emission events/sec across the chip

### Grand Total Estimate

| Event Type | Events/sec (H100) | Events/sec (M4) |
|------------|-------------------|-----------------|
| **Transistor switches** | ~3 x 10^19 | ~2 x 10^19 |
| **Gate tunneling electrons** | ~4 x 10^19 | ~1 x 10^19 |
| **Phonon scattering events** | ~10^23 | ~10^23 |
| **Photon emissions** | ~10^15 | ~10^15 |
| **TOTAL quantum events** | **~10^23** | **~10^23** |

**A modern processor experiences on the order of 10^23 quantum-level events per second** -- a number comparable to a fraction of Avogadro's number. The vast majority of these are phonon scattering events. The "quantum-classical boundary crossings" (where quantum processes produce classical-level signal changes) number approximately **10^19 per second**, corresponding to the transistor switching events and tunneling-leakage electrons.

### Context

At these scales, a single NVIDIA H100 GPU operating at full load produces:
- More quantum tunneling events per second than there are stars in the Milky Way (~10^11)
- More transistor switches per second than the estimated number of grains of sand on Earth (~10^19)
- A total quantum event count comparable to the number of molecules in a few drops of water

Sources: [Transistor Count - Wikipedia](https://en.wikipedia.org/wiki/Transistor_count), [Quantum Effects at 7/5nm and Beyond (Semiconductor Engineering)](https://semiengineering.com/quantum-effects-at-7-5nm/), [Balancing Leakage Currents in Nanometer CMOS Logic (MDPI)](https://www.mdpi.com/2076-3417/11/15/7143)

---

# Summary of Key Numbers

| Quantity | Value | Source/Basis |
|----------|-------|--------------|
| Physical gate oxide (HfO2) thickness | 1.5-2.5 nm | Industry data, IEDM papers |
| Equivalent oxide thickness (EOT) | 0.5-0.8 nm (5nm node) | WikiChip, PMC |
| SiO2 tunneling current at 1.5 nm | ~10-100 A/cm2 | Stanford EE311, Ranuarez et al. |
| Tunneling threshold (SiO2 unusable) | <1.1-1.2 nm | Chenming Hu textbook |
| NVIDIA H100 transistor count | 80 billion | NVIDIA official |
| Apple M4 transistor count | 28 billion | Apple official |
| Tunneling events per transistor | ~10^8-10^10 /sec | Derived from leakage current |
| Hot carrier emission peak | ~1.1 eV (~1100 nm) | ResearchGate, ScienceDirect |
| Hot carrier emission range | 450-1200 nm | IEEE, Nature Photonics |
| Emission quantum efficiency (bulk Si) | <10^-4 | Nature Photonics |
| Si spin qubit T2 (28Si donor) | 2 seconds | Nature Materials |
| Si spin qubit T2 (gate QD, 28Si) | ~100 us | Various |
| Biological coherence (FMO, 277K) | ~300 fs | PNAS |
| Industrial foundry qubit T2 | 4 ms (Hahn echo) | arXiv:2512.20758 |
| Total quantum events (H100) | ~10^23 /sec | Estimated from switching + phonons |
| Quantum-classical transitions (H100) | ~10^19 /sec | Estimated from switching events |

---

# Full Citation List

1. [5nm Process - Wikipedia](https://en.wikipedia.org/wiki/5_nm_process)
2. [3nm Process - Wikipedia](https://en.wikipedia.org/wiki/3_nm_process)
3. [CMOS Scaling for the 5nm Node and Beyond (PMC)](https://pmc.ncbi.nlm.nih.gov/articles/PMC11123950/)
4. [Equivalent Oxide Thickness - WikiChip](https://en.wikichip.org/wiki/equivalent_oxide_thickness)
5. [Ultimate Scaling of High-k Gate Dielectrics (PMC)](https://ncbi.nlm.nih.gov/pmc/articles/PMC5448919)
6. [Stanford EE311 - Thin Dielectrics for MOS Gate](https://web.stanford.edu/class/ee311/NOTES/GateDielectric.pdf)
7. [A Review of Gate Tunneling Current in MOS Devices (McMaster)](https://www.ece.mcmaster.ca/~chihhung/Publication/MR_46_Gate_Tunneling.pdf)
8. [Chenming Hu - MOSFETs in ICs: Scaling, Leakage (Berkeley)](https://www.chu.berkeley.edu/wp-content/uploads/2020/01/Chenming-Hu_ch7.pdf)
9. [NVIDIA Hopper Architecture In-Depth](https://developer.nvidia.com/blog/nvidia-hopper-architecture-in-depth/)
10. [Apple M4 - Wikipedia](https://en.wikipedia.org/wiki/Apple_M4)
11. [Transistor Count - Wikipedia](https://en.wikipedia.org/wiki/Transistor_count)
12. [Quantum Effects at 7/5nm and Beyond (Semiconductor Engineering)](https://semiengineering.com/quantum-effects-at-7-5nm/)
13. [Quantum Mechanics Meets Semiconductors (PBS)](https://www.pbs.org/transistor/science/info/qmsemi.html)
14. [Quantum and Semiconductor Device Physics (UMD)](https://user.eng.umd.edu/~neil/enee307/Quantum_and_Device_Phyiscs_Goldsman_&_%20Darmody.pdf)
15. [Hot-Carrier Luminescence in Si (ResearchGate)](https://www.researchgate.net/publication/13289792_Hot-carrier_luminescence_in_Si)
16. [Watching Chips Work: Picosecond Hot Electron Light Emission (ScienceDirect)](https://www.sciencedirect.com/science/article/abs/pii/S0022024899007046)
17. [Silicon Coupled with Plasmon Nanocavity - Bright Visible Hot-Luminescence (Nature Photonics)](https://www.nature.com/articles/nphoton.2013.25)
18. [Study of Hot-Carrier-Induced Photon Emission from 90nm Si MOSFETs (ScienceDirect)](https://www.sciencedirect.com/science/article/abs/pii/S0169433205003892)
19. [Broadband Near-Infrared Emission in Silicon Waveguides (Nature Communications, 2024)](https://www.nature.com/articles/s41467-024-48772-6)
20. [Electron Spin Coherence Exceeding Seconds in High-Purity Silicon (Nature Materials)](https://www.nature.com/articles/nmat3182)
21. [Engineering Long Spin Coherence Times in Silicon (Nature Materials)](https://www.nature.com/articles/s41563-020-0743-3)
22. [Long Coherence Silicon Spin Qubit in 300mm Foundry (arXiv)](https://arxiv.org/abs/2512.20758)
23. [Long-Lived Quantum Coherence in Photosynthetic Complexes (PNAS)](https://www.pnas.org/doi/10.1073/pnas.1005484107)
24. [Quantum Biology Revisited (PMC)](https://pmc.ncbi.nlm.nih.gov/articles/PMC7124948/)
25. [Quantum Tunneling of Ions Through Closed Voltage-Gated Channels (MDPI)](https://www.mdpi.com/2624-960X/1/2/19)
26. [Water and Hydrophobic Gates in Ion Channels (PMC)](https://pmc.ncbi.nlm.nih.gov/articles/PMC6161260/)
27. [Quantum Biological Tunnel Junction for Electron Transfer (Nature Communications)](https://www.nature.com/articles/s41467-019-11212-x)
28. [Balancing Leakage Currents in Nanometer CMOS Logic (MDPI)](https://www.mdpi.com/2076-3417/11/15/7143)
29. [Leakage Current in Sub-Micrometer CMOS Gates (UFRGS)](https://www.inf.ufrgs.br/logics/docman/book_emicro_butzen.pdf)
30. [The Future Transistors (MIT/Nature, 2023)](https://fisherp.mit.edu/wp-content/uploads/2023/09/s41586-023-06145-x.pdf)
31. [Quantum Tunneling Effects in Ultra-Scaled MOSFETs (Physics Journal, 2024)](https://www.physicsjournal.in/archives/2024/vol6issue1/PartB/7-1-47-600.pdf)
32. [Single-Electron Spin Qubits in Silicon for Quantum Computing (Intelligent Computing)](https://spj.science.org/doi/10.34133/icomputing.0115)
33. [Industry-Compatible Silicon Spin-Qubit Unit Cells Exceeding 99% Fidelity (Nature, 2025)](https://www.nature.com/articles/s41586-025-09531-9)
