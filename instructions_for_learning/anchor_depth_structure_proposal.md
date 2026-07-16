# Anchor-Depth Note Structure — Proposal

Status: proposal, not yet adopted into `biological_engineering_paper_agent.md`. Pilot it on one real paper before deciding whether to fold it into the Anchor Project template there, revise it, or drop it.

## Why a separate structure for anchor depth

Survey notes answer "what happened." Anchor-depth notes should answer "what would I need to actually understand or replicate the design" — that calls for structure closer to a build spec than a prose summary. The seven blocks below are meant to sit inside (and expand) the existing Anchor Project template's `## Engineering design`, `## Measurements and data`, and `## Failure modes` sections, plus add one new section (`Protocol sequence`) that template doesn't currently have.

Illustrative examples below are drawn from the already-summarized (paraphrased) findings in `immunoglobin_rearrangement/b_cell_engineering_papers/leonard2025_note.md` — not new text pulled from the source paper.

## 1. Construct map

A linear, ordered list of the insert's parts. This is the most direct way to see "what was actually built" — prose almost always buries the order.

```text
5' homology arm — iCasp9 (inducible suicide gene)
  — P2A linker — anti-HER2 single-chain antibody cassette — 3' homology arm
  (inserted at the IgH heavy chain locus)
```

## 2. Target site & delivery

Exact locus/site (which intron/exon, safe-harbor vs. functional site), nuclease type, and delivery vector. This block needs a targeted re-read of the paper's methods section, not just a general-summary pass — coordinates and vector details shouldn't be guessed.

```text
target_site: IgH heavy chain locus (functional site, not a safe harbor)
nuclease: CRISPR-Cas9
delivery: [needs methods-section confirmation]
```

## 3. Decision points

One block per choice the authors actually had to make, using an options-plus-reason structure so alternatives and the chosen path are both visible at a glance.

```text
step: payload architecture
options:
  - single gene, safety switch added later               # not used
  - iCasp9 + antibody from one integrated cassette  [chosen]
                                                            # one edit does both jobs;
                                                            # avoids needing a second
                                                            # integration event
```

## 4. Protocol sequence

The ordered workflow — the construct map alone doesn't say when things happen or when checks occur.

```text
1. isolate/activate primary B cells (or cell line)
2. deliver nuclease + donor template
3. select/verify edited cells (e.g. indel PCR)
4. expand and characterize function (in vitro assay)
5. move to in vivo model (if applicable)
```

## 5. Key parameters table

Concrete numeric knobs, as a table rather than scattered in prose — often what actually makes a design reproducible or not.

| Parameter | Value | Why it matters |
|---|---|---|
| Trigger dose (AP1903) | 1 nM | Threshold at which edited cells dropped >50-fold by 72h |
| Time to onset | 5–7 hours | How fast the safety switch acts once triggered |

## 6. Verification/readout mapping

Which assay confirms which construct element worked — ties measurements back to specific design pieces instead of leaving them as a general evidence list.

| Construct element | Verified by |
|---|---|
| Locus integration | Junction PCR / sequencing |
| Antibody expression | Secreted-antibody assay, tumor cell-killing assay |
| Kill switch function | Viability drop after trigger exposure |

## 7. Failure/branch points

Where the design could break, using what actually happened rather than only the parts that worked.

```text
branch: kill-switch durability
  expected: triggered cells die and stay dead
  observed: some edited cells became drug-resistant over time
            by upregulating their own survival pathways (e.g. BCL2, PIM1)
  implication: safety switch is not permanent; may need a second,
               independent mechanism or combination therapy
```

## Open question for piloting

Blocks 1, 3, and 7 can be populated reasonably well from an existing summarized note. Blocks 2 and 5 need a second, narrower fetch focused specifically on a paper's methods section — worth testing whether that's worth the extra effort before applying this structure broadly.
