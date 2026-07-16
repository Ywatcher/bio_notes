# Biological Engineering Paper Agent

This note turns the scale-based training idea from Karim et al. 2024, *Deconstructing synthetic biology across scales*, into a practical workflow for reading biological engineering papers with an agent. The goal is not only to summarize papers, but to extract the engineering problem, the design choice, the scale where the intervention acts, the scale where consequences appear, and the measurements that support the claim.

## Core Reading Principle

For each paper, ask:

```text
What was engineered?
What engineering decision was made?
At what biological scale did that decision act?
At what scale did the consequence appear?
What measurement proves the consequence?
What constraint dominated the design?
What related case should this be compared with?
```

A useful note should not stop at "the authors engineered cells" or "CRISPR was used." It should identify the specific design decision and why that decision mattered.

Example:

```text
Weak extraction:
The paper used CRISPR to modify cells.

Better extraction:
The paper changed guide RNA design and delivery conditions to improve target editing efficiency, but the important limitation appeared at the cell-population scale: heterogeneous editing produced mixed edited and unedited cells.
```

## Key Problems To Extract From Each Paper

### 1. Engineered System

Identify the object being engineered.

Questions:

```text
What biological object is modified?
DNA, RNA, protein, pathway, cell, cell-free system, microbial community, tissue, organism?

What host or chassis is used?
E. coli, yeast, mammalian cell, immune cell, plant, cell-free extract, animal model, microbiome?

What is the intended function?
Detect, produce, kill, regulate, repair, deliver, compute, differentiate, survive, secrete, degrade?
```

Output:

```yaml
engineered_system:
host_or_chassis:
intended_function:
application_context:
```

### 2. Main Engineering Decision

Extract the concrete design choice.

Questions:

```text
What did the authors change?
Promoter, coding sequence, guide RNA, enzyme, pathway branch, receptor domain, delivery vector, culture condition, host strain, circuit topology?

Why did they make that choice?
Higher specificity, stronger expression, lower leakiness, better yield, lower toxicity, improved stability, better delivery, easier scale-up?

What alternatives could have been chosen?
```

Output:

```yaml
main_engineering_decision:
rationale:
alternatives:
```

### 3. Intervention Scale

Classify where the engineering decision acts.

Use these scales:

```text
Molecular:
DNA, RNA, protein, metabolite, binding, folding, catalysis, sequence design.

Circuit or network:
Gene regulation, signaling, metabolic flux, feedback, logic, pathway balancing.

Cell or cell-free system:
Growth, burden, toxicity, phenotype, resource use, stress response, heterogeneity.

Biological community:
Tissue, organ, tumor, biofilm, microbiome, organism, ecological environment.

Societal:
Safety, ethics, regulation, deployment, cost, access, manufacturing, public acceptance.
```

Output:

```yaml
intervention_scale:
scale_reason:
```

### 4. Consequence Scale

Identify where the effect of the design choice appears. This is often different from the intervention scale.

Examples:

```text
Molecular edit -> cell toxicity
Promoter strength -> metabolic burden
Enzyme substitution -> pathway flux and product yield
Receptor affinity -> immune-cell exhaustion
Delivery vector -> tissue specificity and organism-level immune response
Gene drive design -> ecological and societal risk
```

Questions:

```text
At what scale did the main result appear?
Was the consequence expected or unexpected?
Did a lower-scale success fail at a higher scale?
Did a higher-scale phenotype reveal a hidden lower-scale problem?
```

Output:

```yaml
consequence_scale:
observed_consequence:
expected_or_unexpected:
```

### 5. Measurements And Evidence

Extract what was measured, not only what was claimed.

Questions:

```text
What measurements support the main claim?
Expression, fluorescence, sequencing, editing rate, growth curve, viability, metabolite titer, yield, binding affinity, flow cytometry, microscopy, animal phenotype?

At what scale was each measurement made?
Does the measurement directly support the conclusion?
What control or comparison was used?
```

Output:

```yaml
key_measurements:
  - measurement:
    scale:
    supports_claim:
    control_or_comparison:
```

### 6. Dominant Constraint

Every biological engineering paper usually has one or two dominant constraints. Extract them explicitly.

Common constraints:

```text
Specificity
Efficiency
Delivery
Toxicity
Resource burden
Noise
Heterogeneity
Evolutionary stability
Host compatibility
Pathway imbalance
Immune response
Scale-up
Manufacturability
Cost
Regulatory risk
Environmental containment
```

Output:

```yaml
dominant_constraint:
secondary_constraints:
constraint_scale:
```

### 7. Failure Modes And Missing Scales

Look for what did not work or was not tested.

Questions:

```text
What failed or remained weak?
What limitation did the authors admit?
What important scale was not studied?
What experiment would be needed next?
What would make this hard to translate outside the paper's system?
```

Output:

```yaml
failure_modes:
missing_scales:
next_experiments:
translation_risks:
```

### 8. Comparison Case

A single paper can sound generic. Comparison reveals the real engineering lesson.

Questions:

```text
What case should this be compared with?
Same tool, different host?
Same host, different tool?
Same goal, different design?
Same molecular success, different cellular or organism-level outcome?
```

Useful comparison categories:

```text
CRISPR knockout vs CRISPRi
Plasmid expression vs genome integration
Constitutive vs inducible expression
Bacterial vs yeast production
Bulk measurement vs single-cell measurement
In vitro/cell-free vs living-cell implementation
Ex vivo cell engineering vs in vivo delivery
Local therapy vs systemic therapy
```

Output:

```yaml
closest_comparison:
comparison_reason:
what_differs:
what_stays_constant:
```

### 9. Skill Trained By The Paper

Each paper should teach a concrete reading or engineering skill.

Examples:

```text
Promoter selection
Guide RNA design
Plasmid architecture
Pathway balancing
Reporter assay interpretation
Dose-response modeling
Flow cytometry interpretation
Growth-burden analysis
Single-cell heterogeneity analysis
Experimental controls
Safety and containment reasoning
Scale-up reasoning
```

Output:

```yaml
skills_trained:
  - skill:
    why_this_paper_teaches_it:
```

### 10. Depth Decision

Do not treat every paper equally. Classify the note depth.

```text
Survey note:
Useful for general field familiarity. Keep to half a page or one page.

Comparison case:
Useful because it contrasts with another design or system. Make a moderate note and comparison table.

Anchor project:
Worth deeper work: scripts, models, data analysis, reproducible figures, or detailed design-build-test-learn reasoning.
```

Output:

```yaml
paper_depth: survey | comparison_case | anchor_project
reason_for_depth:
```

## Proposed Agent Output Schema

```yaml
citation:
  title:
  authors:
  year:
  doi:

paper_depth:
reason_for_depth:

engineered_system:
host_or_chassis:
intended_function:
application_context:

main_engineering_decision:
rationale:
alternatives:

scale_map:
  molecular:
  circuit_or_network:
  cell_or_cell_free:
  biological_community:
  societal:

intervention_scale:
consequence_scale:
observed_consequence:

key_measurements:
  - measurement:
    scale:
    claim_supported:
    control_or_comparison:

constraints:
  dominant:
  secondary:
  scale_where_constraint_appears:

failure_modes:
missing_scales:
translation_risks:

closest_comparison:
comparison_reason:
what_differs:
what_stays_constant:

skills_trained:
next_questions:
```

## Proposed Project Design

A practical project should work as a casebook plus an agent workspace.

```text
biological-engineering-casebook/
    README.md
    roadmap.md
    case_catalog.csv
    skills_matrix.csv

    prompts/
        01_extract_engineered_system.md
        02_scale_map.md
        03_engineering_decision.md
        04_measurements_constraints.md
        05_failure_modes.md
        06_compare_cases.md
        07_depth_decision.md

    schemas/
        paper_case.yaml
        scale_map.yaml
        comparison.yaml
        skill_record.yaml

    cases/
        crispr_knockout_example/
            case.yaml
            overview.md
            scale_map.md
            engineering_decisions.md
            measurements.md
            failure_modes.md
            open_questions.md
            references.bib
            data/
            notebooks/
            scripts/
            figures/

    comparisons/
        crispr_knockout_vs_crispri.md
        plasmid_vs_genomic_integration.md
        constitutive_vs_inducible_expression.md
        bacterial_vs_mammalian_circuits.md

    methods/
        plasmid_architecture.md
        promoter_selection.md
        golden_gate.md
        gibson_assembly.md
        reporter_assays.md
        dose_response_modeling.md
        flow_cytometry.md
        experimental_controls.md

    concepts/
        gene_expression.md
        transcriptional_regulation.md
        metabolic_burden.md
        gene_circuit_noise.md
        evolutionary_stability.md
        design_build_test_learn.md

    agent_runs/
        2026-07-16_paper_title_slug/
            input.md
            extracted_case.yaml
            draft_note.md
            critique.md
            final_note.md
```

## Agent Workflow

Use a staged workflow rather than one large prompt.

```text
1. Bibliographic capture
   Identify title, authors, year, DOI, paper type, and field.

2. First-pass extraction
   Extract engineered system, host, intended function, and main claim.

3. Scale mapping
   Fill molecular, circuit/network, cell, biological community, and societal scales.

4. Engineering decision extraction
   Identify the design choice, rationale, alternatives, and trade-offs.

5. Evidence extraction
   List measurements, controls, and what each measurement proves.

6. Constraint extraction
   Identify dominant and secondary constraints.

7. Failure and missing-scale analysis
   Identify what was weak, untested, or not translated.

8. Comparison routing
   Suggest which existing case or comparison file this paper belongs with.

9. Depth decision
   Decide survey note, comparison case, or anchor project.

10. Final note generation
   Produce a concise Markdown note plus structured YAML.
```

## Note Depth Templates

### Survey Note

```markdown
# Paper Title

## What was engineered?

## Main engineering decision

## Scale map

## Main measurement

## Dominant constraint

## Closest comparison

## One thing learned

## Questions
```

### Comparison Case

```markdown
# Case Comparison

## Cases compared

## What stays constant?

## What differs?

## Scale where the difference matters

## Measurements that show the difference

## Engineering lesson
```

### Anchor Project

```markdown
# Anchor Project

## Core question

## Biological system

## Engineering design

## Scale map

## Measurements and data

## Reproducible analysis plan

## Model or simulation idea

## Failure modes

## Design-build-test-learn interpretation

## Follow-up papers
```

## Minimal First Version

Start simple. The first version of the project only needs:

```text
case_catalog.csv
skills_matrix.csv
prompts/extract_case.md
schemas/paper_case.yaml
cases/<paper_slug>/overview.md
cases/<paper_slug>/case.yaml
comparisons/<topic>.md
```

The agent's first reliable job should be:

```text
Given one paper, create a structured case note and decide whether it is survey, comparison, or anchor depth.
```

Only after that works should the project add memory across many papers, automatic comparison, data extraction, or script generation.
