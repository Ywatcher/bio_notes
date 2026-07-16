# Immunoglobulin Gene Rearrangement Feedback Loop

Created: 2026-07-16

Starting question: Claverie and Langman modeled immunoglobulin gene rearrangement as a stochastic computer process with a feedback stop condition \cite{ClaverieLangman1984ComputerView}. Newer work keeps the same algorithmic skeleton, but expands the biological implementation: feedback controls receptor signaling, RAG expression, DNA-damage checkpoints, chromatin accessibility, cell cycle, survival, and receptor editing \cite{VettermannSchlissel2010AllelicExclusion}.

Use with BibTeX keys from the nearby file:

`references.bib`

## Pseudocode

```text
initialize developing B cell
initialize IgH, Igk, and Igl loci in their current germ-line/rearranged states
initialize RAG expression, chromatin accessibility, cell-cycle state, and survival state

while cell is alive:

    update developmental signals
    update IL-7, pre-BCR/BCR, PI3K/AKT, ERK, FOXO1, and checkpoint signals
    update RAG1/RAG2 expression and activity
    update chromatin accessibility and 3D locus architecture

    if unresolved RAG-created DNA breaks exist:
        activate ATM-dependent DNA-damage checkpoint
        pause or reduce unsafe proliferation
        inhibit pre-BCR/BCR signaling that would push the cell into cycle
        reduce or reshape RAG activity until breaks are repaired
        continue

    if no RAG activity or cell is not in a permissive state:
        wait, differentiate, die, or receive new developmental signals
        continue

    choose an accessible recombination target:
        early pro-B cell: usually IgH D-J, then IgH V-DJ
        pre-B cell after productive IgH: usually Igk, then Igl if needed
        autoreactive immature B cell: secondary light-chain rearrangement may occur

    perform one V(D)J recombination attempt:
        RAG1/RAG2 bind recombination signal sequences
        RAG creates DNA double-strand breaks
        DNA repair joins gene segments
        result may be productive or nonproductive

    if productive IgH rearrangement produces a usable heavy chain:
        make mu heavy chain
        pair mu with surrogate light chain
        assemble pre-BCR
        signal through Ig-alpha/Ig-beta, SYK, BLNK, BTK, PLC, ERK, and PI3K/AKT
        suppress further IgH rearrangement
        alter RAG expression and chromatin accessibility
        proliferate after DNA breaks are resolved
        progress toward light-chain rearrangement
        continue

    if productive light-chain rearrangement produces usable Igk or Igl:
        pair light chain with heavy chain
        assemble complete BCR

        if BCR is acceptable and not strongly self-reactive:
            PI3K/AKT signaling suppresses FOXO1-driven Rag1/Rag2 expression
            reduce recombination accessibility at Ig loci
            stop rearrangement
            mature
            break

        if BCR is self-reactive:
            maintain or reinduce RAG activity
            perform secondary light-chain rearrangement
            attempt receptor editing
            continue

    if rearrangement is nonproductive:
        if unused alleles or secondary rearrangement substrates remain:
            continue
        else:
            fail selection, die, or remain nonfunctional/null
            break
```

## Algorithmic Interpretation

Claverie and Langman's model can be read as a stochastic state-transition algorithm: each cell moves through possible Ig-locus configurations, chooses rearrangement targets with unequal probabilities, and exits when a functional immunoglobulin product appears \cite{ClaverieLangman1984ComputerView}. In modern terms, the "exit condition" is not a single boolean. It is a set of coupled tests:

- Was the DNA join productive?
- Was a stable protein produced?
- Did the protein assemble into pre-BCR or BCR?
- Did receptor signaling occur?
- Are RAG-created DNA breaks repaired?
- Is the receptor tolerated, or does it trigger receptor editing?
- Are the relevant loci still accessible to RAG?

This changes the algorithm from "try until productive, then stop" into a regulated state machine. Productive rearrangement, DNA damage, receptor signaling, and self-reactivity all feed back into the next possible operation.

## Biological and Chemical Concepts

### RAG1/RAG2

RAG1 and RAG2 are the lymphocyte-specific recombinase proteins that initiate V(D)J recombination. They recognize recombination signal sequences next to V, D, and J gene segments and cut the DNA. Without RAG activity, the cell cannot assemble antigen-receptor variable-region exons.

Newer work shows that RAG expression is actively regulated, not merely present or absent. FOXO1 directly promotes Rag1/Rag2 transcription \cite{AminSchlissel2008Foxo1Rag}. AKT signaling suppresses FOXO1 activity, so PI3K/AKT signaling can reduce Rag expression \cite{Lazorchak2010Sin1mTORC2}. Other regulators, including ERK/E47 and Gfi1b, also tune Rag transcription \cite{Novak2010MAPKPI3KRag,Schulz2012Gfi1bRag}.

### RAG-Created DNA Breaks

"RAG-created DNA breaks" are double-strand DNA breaks made during recombination. They are necessary intermediates: the cell must cut DNA before it can join V, D, and J segments. They are also dangerous, because double-strand breaks can cause chromosomal translocations or genome instability if the cell proliferates before repair.

Later work shows that these breaks are themselves feedback signals. RAG-mediated breaks activate ATM-dependent DNA-damage checkpoints. One pathway induces SPIC through ATM and NF-kB2, which suppresses SYK and BLNK and thereby inhibits pre-BCR signaling while breaks are present \cite{Bednarski2016RAGDSBCheckpoint}. Another study showed DNA damage can downregulate RAG1/2 through ATM-FOXO1 signaling \cite{OchodnickaMackovicova2016DNARepairRag}. This adds an intermediate checkpoint not present in the simple 1984 model.

### Productive vs Nonproductive Rearrangement

A productive rearrangement preserves an open reading frame and can produce a usable immunoglobulin chain. A nonproductive rearrangement may be out of frame, contain stop codons, or fail to produce a stable protein. In the original stochastic model, productive rearrangement is the main success condition \cite{ClaverieLangman1984ComputerView}.

Modern work keeps this distinction, but adds more downstream tests. A rearrangement can be genetically productive yet still fail if the protein does not fold, pair, signal, or pass tolerance.

### Heavy-Chain Feedback and the Pre-BCR

After productive IgH rearrangement, the cell makes a mu heavy chain. The mu chain pairs with surrogate light-chain components to form the pre-B-cell receptor, or pre-BCR. The pre-BCR signals that heavy-chain rearrangement succeeded.

This signal promotes developmental progression and helps suppress further heavy-chain rearrangement, producing heavy-chain allelic exclusion. However, maturation/expansion and allelic exclusion are not identical outputs of one simple pathway; they can be experimentally separated \cite{Iritani1999DistinctSignals}. Pre-BCR signaling components such as PLCgamma1 contribute to early B-cell development and allelic exclusion \cite{Wen2004PLCgamma1}.

### Ig-alpha/Ig-beta, SYK, BLNK, BTK, PLC, ERK

The pre-BCR and BCR signal through associated signaling proteins. Ig-alpha/Ig-beta are receptor-associated signaling chains. SYK is a tyrosine kinase. BLNK is an adaptor protein. BTK and PLC participate downstream in signal propagation. ERK is part of the MAPK pathway.

In algorithmic language, these pathways convert "a protein assembled successfully" into changes in cell state: survival, proliferation, differentiation, RAG expression, and locus accessibility. The Bednarski et al. checkpoint study is especially useful because it shows that RAG DNA breaks can suppress the SYK/BLNK arm of pre-BCR signaling through SPIC while DNA is broken \cite{Bednarski2016RAGDSBCheckpoint}.

### PI3K/AKT/FOXO1

PI3K/AKT/FOXO1 is one of the clearest biochemical feedback routes from receptor signaling to recombination control.

FOXO1 promotes Rag1/Rag2 transcription \cite{AminSchlissel2008Foxo1Rag,Dengler2008Foxo1BCell}. PI3K activates AKT, and AKT inhibits FOXO1 activity. Therefore, strong PI3K/AKT signaling tends to lower RAG expression. This links receptor signaling to the stop/reduce-recombination part of the algorithm \cite{Lazorchak2010Sin1mTORC2,Coffre2016miRNAReceptorEditing,Benhamou2018MycMiR1792PTEN}.

### Chromatin Accessibility

RAG proteins do not cut every possible site equally. The locus must be accessible. Accessibility is controlled by chromatin state, histone marks, enhancers, transcription, and 3D genome organization.

BRWD1 is an example of a chromatin reader that helps target and restrict recombination to the Igk locus in small pre-B cells \cite{Mandal2015BRWD1Igk}. This means feedback can alter the probability that a locus is chosen, not only whether recombination is globally on or off.

### 3D Locus Architecture

Immunoglobulin loci are large genomic regions. Distant V genes must physically encounter D/J or J regions to recombine. This depends on looping, extrusion, and locus folding. Newer work shows that Igh and Igk use different folding principles in pro-B and pre-B cells \cite{Hill2023IghIgkFolding}.

Algorithmically, 3D architecture changes the accessible target set. The cell is not sampling all V genes uniformly; it samples from a changing physical configuration.

### Receptor Editing

Receptor editing is secondary rearrangement, usually at light-chain loci, after a BCR forms but proves self-reactive. In that case, functional receptor expression is not enough to stop the process. The cell can maintain or reinduce RAG and attempt another light-chain rearrangement to replace the autoreactive specificity.

The PI3K/AKT/FOXO1 axis helps regulate this editing state \cite{Coffre2016miRNAReceptorEditing}. Recent work on Igk shows a more detailed mechanism for secondary rearrangements: after primary Igk rearrangement, linear RAG scanning can mediate editing of Igk variable-region repertoires \cite{Li2026LinearRAGScanning}.

## Compact Model

The updated model can be summarized as:

```text
recombination attempt
    -> DNA-break checkpoint feedback
    -> protein assembly test
    -> receptor signaling feedback
    -> RAG transcription/activity feedback
    -> chromatin/accessibility feedback
    -> tolerance/receptor-editing feedback
    -> next state
```

The 1984 paper captured the computational shape: stochastic rearrangement with biased target choice and feedback stop conditions \cite{ClaverieLangman1984ComputerView}. Later work supplies the biochemical machinery behind those conditions.
