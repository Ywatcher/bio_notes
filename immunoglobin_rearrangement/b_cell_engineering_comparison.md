# B Cell Engineering: Stage Comparison

## Why this note exists

Core question driving this note: of the full BCR rearrangement/production process, which stages have actually been engineered, and what happened when they were? Source material is the `b_cell_engineering_2026-07-16` rough-sources bucket (10 papers), read at bibliography level (title, journal, one-line annotation) — no full text was available at the time of writing, so this is a batch/candidate-level comparison, not anchor-project depth on any single paper.

## Stages of the BCR rearrangement/production pipeline

1. **Locus accessibility** — chromatin state controlling which V/D/J segments are available for recombination \cite{Mandal2015BRWD1Igk, Hill2023IghIgkFolding}.
2. **V(D)J recombination** — RAG-mediated cleavage and rejoining at recombination signal sequences; generates initial receptor diversity \cite{ClaverieLangman1984ComputerView, Xu2023SpiCPu1RagIgk}.
3. **DNA-damage checkpoint/repair** — sensing RAG-induced double-strand breaks, gating progression to the next stage \cite{Bednarski2016RAGDSBCheckpoint, OchodnickaMackovicova2016DNARepairRag}.
4. **Pre-BCR/BCR checkpoint & allelic exclusion** — testing the first successful rearrangement, shutting down further rearrangement on the other allele \cite{VettermannSchlissel2010AllelicExclusion, Iritani1999DistinctSignals, Wen2004PLCgamma1}.
5. **Receptor editing** — secondary rearrangement if the receptor is self-reactive or non-functional \cite{Coffre2016miRNAReceptorEditing, Li2026LinearRAGScanning}.
6. **Peripheral selection** — naive B cell output.
7. **Germinal center reaction** — somatic hypermutation (SHM) and affinity maturation \cite{VictoraNussenzweig2022GerminalCenters}.
8. **Class switch recombination (CSR)** \cite{Xu2005DNALesionsCSRSHM}.
9. **Terminal differentiation** — memory B cell or plasma cell, long-term secretion \cite{Budeus2023HumanIgMMemoryBCells}.

## Papers mapped to stages

| Paper | Stage(s) engineered | Outcome |
|---|---|---|
| \cite{Nahmad2020EngineeredBCellsAntiHIV} | 2 (BCR knock-in) → relies on natural 7–9 | Engineered anti-HIV BCR still enters germinal centers, undergoes CSR, clonal expansion, memory retention |
| \cite{Huang2020VaccineElicitationEngineeredBCells} | 2 (BCR knock-in) → natural 7 | Vaccination triggers SHM/affinity maturation on the engineered starting BCR |
| \cite{Kuhlmann2025IgHLocusEditingMacaqueBCells} | 2 (IgH locus editing), primate model | Confers antibody production in vivo — translational step beyond mouse |
| \cite{Leonard2025ICasp9GenomeEditedBCells} | 2 (specificity redirection) + a synthetic safety layer (iCasp9) outside the natural stages | Controllable growth/function of edited B cells |
| \cite{Boucher2025EngineeredHumanBCellsTumorAntigens} | 2 (BCR retargeted to tumor antigens) | Antigen presentation + antibody-mediated function in human B cells |
| \cite{Johnson2018EngineeringPrimaryHumanBCells}, \cite{Laoharawee2020CRISPRPrimaryHumanBCells} | Tooling for stage 2 | Method/protocol enabling efficient editing; not a biological outcome itself |
| \cite{Majors2012DirectedEvolutionBySHM} | 7 specifically — hijacks the SHM machinery *outside* B cells entirely | Directed evolution of a non-antibody protein (Bcl-xL) |
| \cite{Liu2025EngineeredBCellFactorIX} | 9 only, doesn't touch 1–8 | Sustained secretion of a therapeutic protein (Factor IX) |
| \cite{Hill2026EngineeredPlasmaCellBiology} | 9, with BCR display (adjacent to 2/7) used as a screening tool | In vivo evaluation of engineered plasma cell persistence/secretion |

## Observations

Nearly all "B cell engineering" papers intervene at **stage 2** (swap in a defined BCR) and then let the natural downstream machinery (7–9) carry out affinity maturation, class switching, and memory formation — none of them re-engineer the rearrangement *mechanism* itself (RAG activity, checkpoint logic at stage 3, or allelic exclusion at stage 4). \cite{Majors2012DirectedEvolutionBySHM} is the outlier: it engineers a *component* of the machinery (SHM) directly, applied outside the B-cell/antibody context altogether. \cite{Liu2025EngineeredBCellFactorIX} and \cite{Hill2026EngineeredPlasmaCellBiology} skip the generative process (stages 1–8) entirely and use B/plasma cells only as a secretion chassis downstream of it.

## Open questions

- Stages 1, 3, 4, 5, 6, and 8 have no direct engineering example in this batch — is that because they are harder to intervene on, less useful therapeutically, or simply not yet searched for?
- Would engineering stage 3 (the DNA-damage checkpoint) or stage 4 (allelic exclusion) directly be feasible, or does perturbing them risk genomic instability given their safety-gating role?
- This comparison is at bibliography-note depth only — promoting any one paper (e.g. \cite{Nahmad2020EngineeredBCellsAntiHIV} + \cite{Huang2020VaccineElicitationEngineeredBCells} as a comparison case, or \cite{Majors2012DirectedEvolutionBySHM} as an anchor project) would require reading full text to verify these stage assignments and add real measurements.

## Relation to other notes in this topic

Stages 1–5 correspond to the feedback/checkpoint mechanisms already covered in `feedback_loop_note.md` and `bcr_loop_substances_and_processes.md`. Stages 7–9 correspond to the germinal center/memory material referenced via \cite{VictoraNussenzweig2022GerminalCenters}, not yet given its own dedicated note in this topic.
