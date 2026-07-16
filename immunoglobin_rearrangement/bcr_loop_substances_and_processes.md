# BCR Loop: Key Substances and Processes

Created: 2026-07-16

Purpose: summarize the biological substances, molecular reactions, and feedback processes that make the B-cell receptor (BCR) rearrangement loop work. This note complements the algorithm-oriented notes on the 1984 stochastic model and newer feedback mechanisms \cite{ClaverieLangman1984ComputerView,VettermannSchlissel2010AllelicExclusion}.

## Big Picture

The BCR loop is a biological generate-test-select process.

```text
make DNA rearrangement
-> produce immunoglobulin chain
-> assemble pre-BCR or BCR
-> signal success, failure, damage, or self-reactivity
-> continue, stop, edit, proliferate, mature, or die
```

The loop works because the cell links molecular events at DNA, RNA, protein, receptor, and signaling levels.

## Main Molecular Parts

### Immunoglobulin gene loci

The main loci are:

```text
Igh: immunoglobulin heavy-chain locus
Igk: immunoglobulin kappa light-chain locus
Igl: immunoglobulin lambda light-chain locus
```

Each locus contains gene segments:

```text
V = variable segment
D = diversity segment, used in heavy chain
J = joining segment
C = constant region
```

Heavy-chain rearrangement uses:

```text
V_H + D_H + J_H -> VDJ exon
```

Light-chain rearrangement uses:

```text
V_K + J_K -> VJ exon
V_L + J_L -> VJ exon
```

Algorithmic role: these gene segments are the modular parts that can be recombined to generate receptor diversity.

### Recombination signal sequences

Recombination signal sequences, or RSSs, flank V, D, and J segments. RAG proteins recognize RSSs and use them to decide where to cut.

Algorithmic role:

```text
RSS = address tag for recombination machinery
```

Without proper RSS organization, RAG does not efficiently rearrange the segment.

### RAG1 and RAG2

RAG1/RAG2 are the recombinase proteins that initiate V(D)J recombination. They bind RSSs, bring compatible segments together, and cut DNA.

Algorithmic role:

```text
RAG = genome-editing operator for the rearrangement step
```

RAG expression is controlled by developmental signals and transcription factors, especially FOXO1 \cite{AminSchlissel2008Foxo1Rag,Dengler2008Foxo1BCell}. PI3K/AKT signaling can suppress FOXO1 activity and thereby reduce Rag1/Rag2 expression \cite{Lazorchak2010Sin1mTORC2,Coffre2016miRNAReceptorEditing}.

### TdT and junctional diversity enzymes

Terminal deoxynucleotidyl transferase, or TdT, can add non-templated nucleotides at coding joins. DNA polymerases and nucleases can also add or remove bases near the junction.

Algorithmic role:

```text
junction modification = local randomization of sequence
```

This increases diversity, especially in the CDR3 region, but also increases the chance of out-of-frame or nonproductive joins.

### DNA repair machinery

After RAG cuts DNA, the cell must repair the coding ends through non-homologous end joining (NHEJ). Important components include DNA-PK, Ku proteins, Artemis, XRCC4, DNA ligase IV, and related repair factors.

Algorithmic role:

```text
DNA repair = join operation that converts broken segments into a rearranged exon
```

If repair fails or joins the wrong ends, the result can be nonproductive or dangerous.

### Heavy chain, surrogate light chain, and pre-BCR

A productive heavy-chain rearrangement can produce a mu heavy chain. Before a real light chain is made, the mu chain pairs with surrogate light-chain components to form the pre-BCR.

```text
mu heavy chain + surrogate light chain + Ig-alpha/Ig-beta -> pre-BCR
```

Algorithmic role:

```text
pre-BCR = test device for heavy-chain usability
```

If the heavy chain cannot form a pre-BCR, the cell does not receive the proper progression signal.

### Light chain and complete BCR

After light-chain rearrangement, a productive kappa or lambda light chain can pair with the heavy chain.

```text
heavy chain + light chain + Ig-alpha/Ig-beta -> BCR
```

Algorithmic role:

```text
BCR = test device for complete receptor usability and tolerance
```

The complete BCR allows the immature B cell to test whether the receptor is expressed, signals, and is not strongly self-reactive.

### Ig-alpha and Ig-beta

Ig-alpha and Ig-beta, encoded by CD79A and CD79B, are signaling proteins associated with pre-BCR and BCR complexes. The immunoglobulin chains bind antigen or assemble structurally; Ig-alpha/Ig-beta transmit signals into the cell.

Algorithmic role:

```text
Ig-alpha/Ig-beta = signal transduction adapter for receptor state
```

## Recombination Process

### Step 1: Locus becomes accessible

A locus must be in a recombinationally accessible chromatin state. Accessibility depends on transcription, enhancers, histone marks, chromatin readers, nuclear location, and 3D folding.

```text
closed chromatin -> low recombination probability
open chromatin -> high recombination probability
```

BRWD1 helps target recombination to Igk in small pre-B cells \cite{Mandal2015BRWD1Igk}. 3D architecture also matters because distant V segments must physically contact recombination centers \cite{Hill2023IghIgkFolding}.

### Step 2: RAG binds RSSs

RAG1/RAG2 bind recombination signal sequences adjacent to gene segments. Compatible RSSs are paired according to recombination rules.

Algorithmically:

```text
candidate targets = accessible segments with compatible RSSs
```

### Step 3: DNA is cut

RAG introduces DNA breaks at the boundary between coding sequence and RSS. This creates coding ends and signal ends.

Biological meaning:

```text
RAG-created double-strand break = necessary but dangerous intermediate
```

Algorithmic role:

```text
state = unresolved_DNA_breaks
```

### Step 4: Coding ends are processed

Hairpin coding ends are opened. Bases may be deleted. P-nucleotides and N-nucleotides may be added. TdT contributes to non-templated additions.

Algorithmic role:

```text
junction_sequence = edited/randomized local join sequence
```

This is a major source of diversity and nonproductive outcomes.

### Step 5: DNA repair joins the ends

NHEJ ligates the processed ends to form a rearranged exon.

```text
V + D + J -> heavy-chain variable exon
V + J -> light-chain variable exon
```

Algorithmic role:

```text
new candidate sequence is produced
```

### Step 6: Sequence is tested through expression

The rearranged gene is transcribed and translated. If it is in frame and lacks premature stop codons, it may produce a protein chain.

```text
productive DNA join -> mRNA -> protein chain
nonproductive DNA join -> no usable chain
```

The 1984 model compressed this into a productive-fusion probability, around 0.3 for light chains \cite{ClaverieLangman1984ComputerView}.

## Feedback Processes

### DNA-damage feedback

RAG-created DNA breaks activate DNA-damage responses. ATM is a central checkpoint kinase. This checkpoint helps prevent the cell from proliferating while DNA is broken.

One described pathway:

```text
RAG break -> ATM -> NF-kB2/SPIC -> reduced SYK and BLNK -> reduced pre-BCR signaling
```

This inhibits unsafe pre-BCR-driven cycling while DNA breaks are present \cite{Bednarski2016RAGDSBCheckpoint}. DNA damage can also downregulate RAG1/2 through ATM-FOXO1 signaling \cite{OchodnickaMackovicova2016DNARepairRag}.

Algorithmic role:

```text
if unresolved_DNA_breaks:
    pause proliferation
    activate repair
    reduce unsafe receptor signaling
    delay further recombination
```

### Heavy-chain success feedback

A productive heavy-chain rearrangement produces mu heavy chain. If mu pairs with surrogate light chain, the pre-BCR forms and signals.

Main effect:

```text
pre-BCR signal -> heavy chain worked
```

Consequences:

```text
increase survival
promote proliferation after repair
suppress further heavy-chain rearrangement
prepare for light-chain rearrangement
```

Pre-BCR signaling uses pathways involving Ig-alpha/Ig-beta, SYK, BLNK, BTK, PLC, ERK, and PI3K/AKT. Some outputs for maturation/proliferation and allelic exclusion can be experimentally separated \cite{Iritani1999DistinctSignals,Wen2004PLCgamma1}.

Algorithmic role:

```text
if preBCR_signal adequate:
    stage = pre_B
    IgH_search = stop_or_reduce
    light_chain_search = prepare
```

### RAG-expression feedback

RAG expression is controlled by transcription factors and signaling pathways.

Core relation:

```text
FOXO1 active -> Rag1/Rag2 high
PI3K/AKT active -> FOXO1 inhibited -> Rag1/Rag2 reduced
```

FOXO1 directly regulates Rag transcription \cite{AminSchlissel2008Foxo1Rag}. mTORC2/AKT2 and PTEN-AKT-FOXO1 pathways tune Rag expression and receptor editing states \cite{Lazorchak2010Sin1mTORC2,Coffre2016miRNAReceptorEditing,Benhamou2018MycMiR1792PTEN}. Other regulators, including ERK/E47 and Gfi1b, also affect Rag transcription \cite{Novak2010MAPKPI3KRag,Schulz2012Gfi1bRag}.

Algorithmic role:

```text
RAG level = recombination permission and rate
```

### Chromatin feedback

Signals change chromatin accessibility at Ig loci. This determines which loci can be rearranged next and how likely each target is.

Examples:

```text
IgH accessible in pro-B stage
Igk becomes accessible in small pre-B stage
Igl can be used if kappa attempts fail or are unsuitable
```

Algorithmic role:

```text
available_targets = accessible loci and reachable gene segments
```

This means feedback changes the search space, not only the stop/go state.

### Light-chain success feedback

A productive light-chain rearrangement produces kappa or lambda light chain. If it pairs with heavy chain, the complete BCR forms.

```text
heavy chain + light chain -> BCR
```

The BCR signals that a complete receptor exists.

Algorithmic role:

```text
if complete_BCR and tolerated:
    reduce RAG
    stop rearrangement
    mature
```

### Self-reactivity feedback and receptor editing

If the complete BCR strongly binds self-antigen, the cell may not mature. Instead, it can reinduce or maintain RAG and attempt secondary light-chain rearrangement.

```text
self-reactive BCR -> RAG activity -> secondary light-chain rearrangement -> new specificity
```

This is receptor editing. The PI3K/AKT/FOXO1 axis participates in regulating editing states \cite{Coffre2016miRNAReceptorEditing}. Recent work describes linear RAG scanning as a mechanism for secondary Igk rearrangements \cite{Li2026LinearRAGScanning}.

Algorithmic role:

```text
if BCR is functional but self-reactive:
    do not simply accept
    attempt local sequence replacement by light-chain editing
```

## Key Reactions and Their Algorithmic Meaning

```text
RAG binding to RSS
-> choose recombination target

RAG DNA cleavage
-> create candidate rearrangement intermediate
-> trigger DNA-damage checkpoint

NHEJ repair
-> produce rearranged exon

transcription and translation
-> test whether DNA sequence can make protein

heavy-chain + surrogate light-chain assembly
-> test heavy-chain usability through pre-BCR

heavy-chain + light-chain assembly
-> test complete receptor usability through BCR

pre-BCR/BCR signaling
-> update survival, proliferation, differentiation, RAG, and chromatin states

self-antigen binding
-> update tolerance state; trigger editing, anergy, deletion, or maturation
```

## Minimal State-Machine View

```text
pro-B cell:
    IgH accessible
    RAG active
    D-J and V-DJ recombination
    DNA damage checkpoint during breaks
    test mu chain through pre-BCR

large pre-B cell:
    pre-BCR signal received
    proliferation after successful heavy-chain checkpoint
    RAG reduced temporarily

small pre-B cell:
    RAG active again
    Igk/IgL accessible
    light-chain recombination
    DNA damage checkpoint during breaks

immature B cell:
    complete BCR expressed
    test self-reactivity
    accepted -> mature
    self-reactive -> edit, anergize, or delete
```

## Why This Loop Works

The loop works because each molecular layer produces information for the next step:

```text
DNA sequence tells whether a chain can be encoded
protein folding tells whether the chain is physically usable
receptor assembly tells whether the chain can participate in pre-BCR/BCR
signaling tells the cell whether to survive, proliferate, stop, or continue
DNA-damage sensing tells the cell whether it is safe to proceed
self-reactivity tells the cell whether the receptor should be kept or edited
```

Thus, the BCR loop is not only a DNA recombination system. It is a coupled biochemical control system that links genome editing, protein quality control, receptor signaling, DNA repair, tolerance, and cell fate.

## Why Recombination Is Targeted to Ig Loci

V(D)J recombination is not aimed at the whole genome equally. It is concentrated at immunoglobulin and T-cell receptor loci because RAG cutting requires a combination of sequence recognition, chromatin accessibility, developmental timing, and 3D genome organization.

### RSS sequence recognition

The most direct targeting mechanism is the recombination signal sequence, or RSS. V, D, and J segments are flanked by RSSs. RAG1/RAG2 recognize these RSSs and cut at the border between the RSS and the coding segment.

A canonical RSS contains:

```text
heptamer - spacer - nonamer
```

The heptamer is adjacent to the coding segment. A simplified consensus representation is:

```text
CACAGTG - 12 or 23 bp spacer - ACAAAAACC
```

The exact sequence can vary, and real RSS strength depends on both the conserved motifs and surrounding sequence context. Still, the key point is that RSSs act like molecular address labels for RAG.

Algorithmic role:

```text
if sequence has usable RSS:
    eligible_for_RAG_binding = true
else:
    eligible_for_RAG_binding = low_or_false
```

### The 12/23 rule

RSS spacers are usually either about 12 base pairs or about 23 base pairs. Productive V(D)J recombination generally pairs one 12-RSS with one 23-RSS.

Algorithmic role:

```text
if target_A.RSS_spacer != target_B.RSS_spacer:
    pair_allowed = true
else:
    pair_allowed = disfavored_or_forbidden
```

Biological role: this helps enforce correct joining order. For example, in the heavy-chain locus it helps favor:

```text
D_H to J_H
then V_H to DJ_H
```

rather than arbitrary joining among all nearby segments.

### Coding-end and signal-end cleavage

RAG cuts at the boundary between the coding segment and the RSS. This generates:

```text
coding ends: gene-segment ends that will be joined into the receptor exon
signal ends: RSS-containing ends that are usually joined or discarded separately
```

Algorithmic role:

```text
RAG_cut(RSS_boundary)
    -> coding_ends_for_receptor_gene
    -> signal_ends_for byproduct / repair
```

This boundary-specific cleavage is one reason the recombination product is structured rather than arbitrary DNA damage.

### Cryptic RSSs and off-target risk

The genome can contain RSS-like sequences outside Ig/TCR loci. These are often called cryptic RSSs. RAG can sometimes bind or cut such sites, especially if they are accessible.

Algorithmic implication:

```text
usable_RSS_like_sequence outside Ig/TCR + accessible chromatin
    -> low-probability off-target RAG cut
```

This is one reason RAG activity is dangerous. Off-target cuts or misjoining between Ig loci and other chromosomes can contribute to translocations and lymphoid malignancy. Therefore, the cell tightly restricts RAG expression, cell-cycle timing, and DNA-damage responses.

### Chromatin accessibility

RSS sequence is not enough. RAG must physically access the DNA. Ig loci become open in specific developmental stages through enhancer activity, transcription, histone modifications, and chromatin remodeling.

A simplified rule:

```text
RSS present + closed chromatin -> low RAG cutting
RSS present + open chromatin -> high RAG cutting
```

This explains why the same genome can contain all Ig loci, but different loci are rearranged at different stages:

```text
pro-B cell:
    Igh accessible

small pre-B cell:
    Igk and/or Igl accessible
```

BRWD1 is one example of a chromatin reader that helps target and restrict recombination to the Igk locus in small pre-B cells \cite{Mandal2015BRWD1Igk}.

### Histone marks and RAG2 binding

RAG2 contains a plant homeodomain (PHD) finger that can bind histone H3 trimethylated at lysine 4, written H3K4me3. H3K4me3 is associated with active chromatin and transcriptional activity.

Algorithmic role:

```text
if RSS present and nearby chromatin has H3K4me3:
    RAG recruitment/activity increases
```

This links sequence recognition to chromatin state: RAG does not only read DNA motifs; it also responds to epigenetic context.

### Enhancers and transcription

Ig enhancers promote locus accessibility and transcription through unrearranged regions. Transcription itself is not the goal, but it correlates with open chromatin and recombinational competence.

Algorithmic role:

```text
developmental enhancer active
    -> locus opens
    -> RSSs become accessible
    -> RAG cutting probability increases
```

Thus, developmental signals change which parts of the genome are searchable by RAG.

### 3D genome architecture and recombination centers

Ig loci are large. V segments can be far from D/J or J segments in linear DNA. Looping and 3D folding bring distant segments into contact with recombination centers where RAG is concentrated.

Algorithmic role:

```text
cut_probability(segment) = f(RSS_strength, accessibility, contact_with_recombination_center)
```

Recent work shows that Igh and Igk use different folding principles for V gene recombination in pro-B and pre-B cells \cite{Hill2023IghIgkFolding}. Secondary Igk rearrangements can involve linear RAG scanning from recombination centers, especially during receptor editing \cite{Li2026LinearRAGScanning}.

### Combined targeting rule

A useful summary is:

```text
RAG targeting probability is high when:
    RSS is recognizable
    RSS pairing obeys recombination rules
    chromatin is accessible
    active histone marks support RAG activity
    developmental enhancers are active
    locus architecture brings segments together
    RAG is expressed in the right cell-cycle/developmental window
```

And low when any of those conditions are missing.

So the system is not targeted by a single mechanism. It is targeted by layered constraints:

```text
sequence motif constraint
+ spacer-pairing constraint
+ chromatin accessibility constraint
+ epigenetic constraint
+ developmental timing constraint
+ 3D contact constraint
+ DNA-damage checkpoint constraint
```

These layered constraints make Ig/TCR loci the normal targets while reducing, but not eliminating, off-target recombination elsewhere in the genome.
