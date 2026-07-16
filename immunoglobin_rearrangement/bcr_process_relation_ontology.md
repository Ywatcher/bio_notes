# BCR Process Relation Ontology

Created: 2026-07-16

Purpose: define structured relation clauses for understanding how the B-cell receptor (BCR) rearrangement, feedback, selection, and engineering processes relate to each other. This note complements the mechanism note and the engineering-stage comparison \cite{VettermannSchlissel2010AllelicExclusion,Bednarski2016RAGDSBCheckpoint,Nahmad2020EngineeredBCellsAntiHIV,Huang2020VaccineElicitationEngineeredBCells}.

## Why an Ontology Helps

The BCR loop is not just a list of stages. It is a linked system of molecular reactions, checkpoint evaluations, feedback controls, developmental state changes, and later engineering interventions.

A useful structure should distinguish:

```text
biological process: what physically happens
algorithmic role: what the process does in generate-test-select terms
engineering relation: how a paper or intervention modifies, bypasses, or preserves the natural loop
```

Example clause:

```yaml
subject: RAG1/RAG2
relation: recognizes
object: RSS
stage: V(D)J recombination
biological_effect: targets DNA cleavage to immunoglobulin gene segments
algorithmic_role: chooses candidate recombination targets
constraint_type: hard-ish targeting rule
```

## Core Entity Types

```text
molecule: RAG1/2, TdT, ATM, FOXO1, SYK, BLNK, Ig-alpha, Ig-beta
sequence_feature: RSS, V segment, D segment, J segment, coding end, signal end
locus: Igh, Igk, Igl
cell_state: pro-B cell, large pre-B cell, small pre-B cell, immature B cell, mature B cell, plasma cell
process: V(D)J recombination, NHEJ repair, receptor editing, SHM, CSR
checkpoint: DNA-damage checkpoint, pre-BCR checkpoint, BCR tolerance checkpoint
signal: pre-BCR signal, BCR signal, self-antigen binding, PI3K/AKT activity
engineering_intervention: BCR knock-in, IgH locus editing, synthetic safety switch, SHM hijacking, plasma-cell secretion chassis
paper: cited engineering or mechanism work
```

## Relation Class 1: Material and Biochemical Relations

These describe what physically happens.

Useful predicates:

```text
recognizes
binds
cleaves
joins
adds
deletes
repairs
transcribes_to
translates_to
assembles_with
expressed_as
```

Examples:

```yaml
- subject: RAG1/RAG2
  relation: recognizes
  object: RSS
  stage: V(D)J recombination
  biological_effect: identifies legal recombination targets near V, D, and J segments
  algorithmic_role: target-address recognition
  constraint_type: hard-ish

- subject: RAG1/RAG2
  relation: cleaves
  object: DNA at RSS-coding boundary
  stage: V(D)J recombination
  biological_effect: creates coding ends and signal ends
  algorithmic_role: starts generation of a candidate receptor sequence
  constraint_type: dangerous necessary intermediate

- subject: NHEJ machinery
  relation: joins
  object: processed coding ends
  stage: DNA repair after RAG cleavage
  biological_effect: creates a rearranged VDJ or VJ exon
  algorithmic_role: converts broken intermediate into candidate sequence
  constraint_type: hard repair requirement

- subject: mu heavy chain
  relation: assembles_with
  object: surrogate light chain and Ig-alpha/Ig-beta
  stage: pre-BCR checkpoint
  biological_effect: forms pre-BCR
  algorithmic_role: tests whether heavy-chain candidate is physically usable
  constraint_type: hard gate
```

## Relation Class 2: Stage and Pipeline Relations

These locate a process in the BCR production pipeline.

Useful predicates:

```text
occurs_in_stage
precedes
follows
requires
enables
gates
terminates
branches_to
```

Examples:

```yaml
- subject: locus accessibility
  relation: precedes
  object: V(D)J recombination
  biological_effect: makes Ig segments available to RAG
  algorithmic_role: defines the current search space
  constraint_type: soft-to-hard depending on accessibility

- subject: DNA-damage checkpoint
  relation: gates
  object: proliferation and further recombination
  biological_effect: prevents progression while RAG-created breaks are unresolved
  algorithmic_role: safety gate
  constraint_type: hard safety constraint

- subject: productive heavy-chain rearrangement
  relation: enables
  object: pre-BCR signaling
  biological_effect: allows mu heavy chain to be tested with surrogate light chain
  algorithmic_role: candidate passes first expression/assembly test
  constraint_type: hard checkpoint

- subject: self-reactive BCR detection
  relation: branches_to
  object: receptor editing, anergy, deletion, or maturation failure
  biological_effect: prevents strong self-reactive receptors from entering the mature pool
  algorithmic_role: negative selection and local retry
  constraint_type: tolerance gate
```

## Relation Class 3: Evaluation and Checkpoint Relations

These describe how a candidate result is tested.

Useful predicates:

```text
evaluates
detects
passes
fails
selects_for
selects_against
triggers_editing
triggers_deletion
triggers_survival
```

Examples:

```yaml
- subject: reading frame and stop codon status
  relation: evaluates
  object: rearranged DNA sequence
  stage: expression test
  biological_effect: determines whether the rearranged exon can produce a usable protein chain
  algorithmic_role: hard validity check on generated sequence
  constraint_type: hard

- subject: pre-BCR signal
  relation: evaluates
  object: heavy-chain usability
  stage: heavy-chain checkpoint
  biological_effect: tests whether mu chain assembles and signals through Ig-alpha/Ig-beta
  algorithmic_role: accept heavy-chain candidate, suppress further IgH search, prepare light-chain search
  constraint_type: hard gate with signal threshold

- subject: complete BCR expression
  relation: evaluates
  object: heavy-chain and light-chain compatibility
  stage: immature B-cell checkpoint
  biological_effect: confirms that the receptor can be displayed on the cell surface
  algorithmic_role: candidate receptor becomes testable against antigen/self-antigen
  constraint_type: hard gate

- subject: self-antigen binding
  relation: evaluates
  object: tolerance of complete BCR
  stage: immature B-cell selection
  biological_effect: strong self-reactivity can trigger editing, anergy, or deletion
  algorithmic_role: reject or modify harmful candidates
  constraint_type: hard or soft depending on signal strength and context

- subject: germinal center selection
  relation: selects_for
  object: improved antigen binding after SHM
  stage: affinity maturation
  biological_effect: clones with better antigen capture and T-cell help expand
  algorithmic_role: performance-biased selection after mutation
  constraint_type: soft selection with competitive amplification
```

## Relation Class 4: Feedback and Control Relations

Feedback relations explain how the result of one step changes future search, stopping, survival, or risk control.

Useful predicates:

```text
activates
inhibits
upregulates
downregulates
pauses
restarts
changes_rate_of
changes_search_space
changes_stop_condition
changes_selection_pressure
```

Important feedback-effect types:

```text
changes_search_space: chromatin accessibility changes which loci or segments can be used
changes_operator_rate: RAG level changes recombination frequency
changes_stop_condition: pre-BCR/BCR signaling stops further rearrangement
changes_selection_pressure: antigen or self-antigen binding changes survival, editing, or death
changes_safety_state: DNA-damage response pauses unsafe cycling or recombination
```

Examples:

```yaml
- subject: RAG-created DNA break
  relation: activates
  object: ATM checkpoint
  stage: DNA-damage feedback
  biological_effect: detects unresolved double-strand breaks
  algorithmic_role: prevents continuing the search while the genome is unsafe
  feedback_effect: changes_safety_state
  citation: \cite{Bednarski2016RAGDSBCheckpoint}

- subject: ATM signaling
  relation: downregulates
  object: unsafe pre-BCR signaling and Rag expression
  stage: DNA-damage feedback
  biological_effect: reduces proliferation or recombination pressure during repair
  algorithmic_role: pause and repair before further iteration
  feedback_effect: changes_operator_rate
  citation: \cite{Bednarski2016RAGDSBCheckpoint,OchodnickaMackovicova2016DNARepairRag}

- subject: pre-BCR signal
  relation: inhibits
  object: further heavy-chain recombination
  stage: allelic exclusion
  biological_effect: suppresses additional IgH rearrangements after one usable heavy chain appears
  algorithmic_role: stop searching once a heavy-chain candidate passes
  feedback_effect: changes_stop_condition
  citation: \cite{VettermannSchlissel2010AllelicExclusion,Iritani1999DistinctSignals}

- subject: PI3K/AKT activity
  relation: inhibits
  object: FOXO1-dependent Rag1/Rag2 transcription
  stage: RAG-expression feedback
  biological_effect: lowers RAG availability
  algorithmic_role: reduces recombination permission and rate
  feedback_effect: changes_operator_rate
  citation: \cite{AminSchlissel2008Foxo1Rag,Lazorchak2010Sin1mTORC2,Coffre2016miRNAReceptorEditing}

- subject: self-reactive BCR signal
  relation: restarts
  object: light-chain recombination
  stage: receptor editing
  biological_effect: maintains or reinduces RAG and attempts secondary light-chain rearrangement
  algorithmic_role: local retry instead of immediate acceptance
  feedback_effect: changes_search_space
  citation: \cite{Coffre2016miRNAReceptorEditing,Li2026LinearRAGScanning}
```

## Relation Class 5: Hard and Soft Constraints

The loop has both hard gates and soft biases. This distinction is essential because not every evaluation is a single score.

Useful predicates:

```text
hard_gate_for
soft_biases
increases_probability_of
decreases_probability_of
must_satisfy
improves_fitness_under
```

Examples:

```yaml
- subject: RSS compatibility
  relation: hard_gate_for
  object: legal RAG pairing
  biological_effect: RAG recombination strongly depends on compatible RSS organization and the 12/23 rule
  algorithmic_role: restricts allowed moves
  constraint_type: hard-ish sequence rule

- subject: productive reading frame
  relation: hard_gate_for
  object: protein-chain production
  biological_effect: out-of-frame rearrangements fail to produce usable immunoglobulin chain
  algorithmic_role: invalid candidate rejection
  constraint_type: hard

- subject: chromatin accessibility
  relation: soft_biases
  object: segment recombination probability
  biological_effect: open loci and reachable segments are more likely to be used
  algorithmic_role: changes candidate distribution
  constraint_type: soft bias

- subject: 3D locus folding
  relation: soft_biases
  object: V-segment encounter with recombination centers
  biological_effect: physically favors some segment contacts over others
  algorithmic_role: search-space weighting
  constraint_type: soft bias

- subject: antigen capture in germinal center
  relation: improves_fitness_under
  object: clonal expansion
  biological_effect: higher-performing clones receive more help and expand more
  algorithmic_role: performance-weighted selection
  constraint_type: soft selection
```

## Relation Class 6: Engineering Intervention Relations

These connect engineering papers to the natural BCR loop.

Useful predicates:

```text
engineers
replaces
bypasses
preserves
hijacks
adds_synthetic_control
uses_as_chassis
redirects_specificity
```

Examples:

```yaml
- subject: Nahmad2020EngineeredBCellsAntiHIV
  relation: replaces
  object: natural stochastic BCR generation at stage 2
  intervention: knock in defined anti-HIV BCR
  preserves: germinal center reaction, class switching, clonal expansion, memory retention
  bypasses: discovery of starting specificity by natural V(D)J diversity
  algorithmic_role: seed the loop with a designed candidate and let downstream biological selection continue
  citation: \cite{Nahmad2020EngineeredBCellsAntiHIV}

- subject: Huang2020VaccineElicitationEngineeredBCells
  relation: preserves
  object: vaccination-driven germinal center refinement
  intervention: engineered B cells carrying an HIV broadly neutralizing antibody precursor
  biological_effect: immunization can drive engineered cells through SHM and memory/plasma-cell outputs
  algorithmic_role: designed initial state plus natural mutation-selection refinement
  citation: \cite{Huang2020VaccineElicitationEngineeredBCells}

- subject: Leonard2025ICasp9GenomeEditedBCells
  relation: adds_synthetic_control
  object: growth and function of genome-edited B cells
  intervention: inducible caspase-9 safety switch
  biological_effect: external trigger can eliminate or control edited cells
  algorithmic_role: engineered kill switch outside the natural BCR loop
  citation: \cite{Leonard2025ICasp9GenomeEditedBCells}

- subject: Liu2025EngineeredBCellFactorIX
  relation: bypasses
  object: BCR discovery and antigen-recognition function
  intervention: B-lineage or plasma-cell therapeutic protein secretion
  biological_effect: uses B-cell lineage as durable protein-production platform
  algorithmic_role: use terminal output chassis, not the receptor-search algorithm
  citation: \cite{Liu2025EngineeredBCellFactorIX}

- subject: Majors2012DirectedEvolutionBySHM
  relation: hijacks
  object: somatic hypermutation machinery
  intervention: use SHM-like mutation to evolve non-antibody protein Bcl-xL
  biological_effect: applies B-cell diversification logic outside normal antibody specificity generation
  algorithmic_role: mutation operator for directed evolution
  citation: \cite{Majors2012DirectedEvolutionBySHM}
```

## Three Linked Graph Views

A useful knowledge graph for this topic should allow three simultaneous readings.

```text
Biological graph:
    molecule/process -> biochemical relation -> molecule/process

Algorithm graph:
    operation -> evaluation -> feedback -> next search state

Engineering graph:
    paper/intervention -> modifies/bypasses/preserves -> natural BCR stage
```

The same biological event can therefore have multiple meanings.

Example:

```yaml
entity: pre-BCR signaling
biological_graph:
  - mu heavy chain assembles_with surrogate light chain
  - Ig-alpha/Ig-beta transduces signal
algorithm_graph:
  - evaluates heavy-chain usability
  - changes_stop_condition for IgH search
  - prepares light-chain search
engineering_graph:
  - preserved by BCR knock-in strategies that rely on natural downstream maturation
```

## Minimal Clause Template

Use this template when converting the notes into structured statements.

```yaml
- entity_or_paper:
  relation:
  target:
  stage:
  condition:
  biological_effect:
  algorithmic_role:
  constraint_type: hard | soft | feedback | memory | safety
  feedback_effect: changes_search_space | changes_operator_rate | changes_stop_condition | changes_selection_pressure | changes_safety_state | none
  engineering_relation: natural | replaced | bypassed | preserved | hijacked | added_control | none
  citation:
```

## Example Query Patterns

This structure makes later questions easier to ask.

```text
Which mechanisms change the search space?
Which checkpoints are hard gates?
Which signals reduce RAG activity?
Which papers engineer the starting BCR but preserve natural germinal-center selection?
Which papers bypass the receptor-generation algorithm entirely?
Which processes act like mutation operators, selection operators, or stopping rules?
Which biological events function both as chemical reactions and algorithmic feedback?
```

## Summary

For this topic, the most useful ontology is not a single hierarchy. It is a typed relation system.

The same event should be representable at three levels:

```text
RAG cuts DNA
-> biological reaction: cleavage at RSS-coding boundary
-> algorithmic role: create candidate rearrangement intermediate
-> feedback risk: activates DNA-damage checkpoint
```

Likewise, an engineering paper should be represented by what part of the natural loop it changes:

```text
BCR knock-in
-> replaces natural stochastic starting candidate
-> preserves downstream SHM, CSR, memory, or plasma-cell differentiation
```

This keeps the notes from flattening the BCR system into a simple pathway and instead preserves the generate-test-feedback-select logic that made the process useful to compare with computation.
