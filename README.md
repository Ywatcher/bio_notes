# note1

`note1` is a working note library for papers and topics that are still being understood. The goal is not to make a polished literature review immediately. The goal is to preserve context: what was read, why it was read, what questions were asked, what the agent helped infer, and which sources support the notes.

## Current Topics

- `immunoglobin_rearrangement/`: notes around Claverie and Langman's 1984 stochastic model of immunoglobulin gene rearrangement, plus later sources on feedback loops, RAG regulation, DNA-damage checkpoints, chromatin accessibility, and receptor editing.

Future topics can be added as sibling folders:

```text
note1/
    immunoglobin_rearrangement/
    another_topic/
    another_paper_cluster/
```

## Workflow With An Agent

I keep the agent workspace in another directory, not directly inside `note1`. This is intentional: the agent can inspect and discuss files, but edits to `note1` require explicit permission. That gives a useful safety boundary. The agent should not silently reorganize or overwrite notes.

Typical workflow:

1. Put a paper, PDF, or rough source folder into a topic directory.
2. Ask the agent to read the paper or extract text.
3. Ask conceptual questions in plain language.
4. Ask the agent to search for follow-up work when the question needs newer sources.
5. Ask the agent to create or append Markdown notes with citations.
6. Keep rough sources separate from polished notes.
7. Commit Markdown, BibTeX, and small metadata files; avoid committing PDFs or other large files unless deliberately wanted.

## What To Provide

Useful inputs for the agent:

- The local path to a paper or folder.
- The title or DOI if the paper is not local.
- The specific question to answer.
- Whether to search newer literature.
- Whether the output should be a short explanation, a detailed note, pseudocode, or a bibliography.
- Whether the note should be appended to an existing file or created as a separate note.

Example:

```text
Please read note1/immunoglobin_rearrangement/<paper>.pdf.
Explain the model as an algorithm.
Create a Markdown note with pseudocode and BibTeX-style citations.
Do not move or commit the PDF.
```

## Prompts That Work Well

For first-pass understanding:

```text
What does this paper say? Summarize the central claim, model, assumptions, and conclusions.
```

For algorithmic interpretation:

```text
Translate the paper's model into pseudocode. Separate biological interpretation from algorithmic interpretation.
```

For mechanism follow-up:

```text
Search for newer work that updates this mechanism. Make a rough source folder and a BibTeX file for papers that are useful but not yet carefully read.
```

For note creation:

```text
Create a Markdown note in this topic folder. Include:
- short context
- key concepts
- pseudocode
- biological mechanism
- algorithmic interpretation
- open questions
- TeX-style citations matching references.bib
```

For appending:

```text
Append a new section below the previous note about hard and soft constraints. Explain the biological process and how each constraint changes the algorithm.
```

For source hygiene:

```text
If you fetched papers, do not leave them mixed with notes. Put PDFs or metadata into a rough_sources/<topic>_<date>/ folder, or create references.bib if no PDFs were downloaded.
```

For committing:

```text
Commit the note directory, excluding PDFs and other large files.
```

## Target Note Structure

Topic folders should generally contain:

```text
topic_name/
    README.md                    optional topic overview
    references.bib               bibliography for notes in this topic
    main_note.md                 synthesis or current understanding
    paper_specific_note.md       focused note for one paper
    rough_sources/
        topic_or_question_date/
            README.md
            source-notes.md
            references.bib
            copied_or_rough_notes.md
```

Good note sections:

- Source and citation key
- Why this source was read
- Core question
- Main claim
- Model or mechanism
- Pseudocode, if useful
- Biological/chemical interpretation
- Algorithmic/computational interpretation
- Hard constraints and soft constraints
- Open questions
- Relation to later work

## Citation Style

Use TeX-style citation keys in Markdown:

```text
This model treats rearrangement as a stochastic state-transition process \cite{ClaverieLangman1984ComputerView}.
```

Keep the matching entries in `references.bib` in the same topic folder when possible.

## Git Hygiene

The repository is meant to track notes and source metadata, not large paper files by default.

Track:

- Markdown notes
- BibTeX files
- README files
- small source summaries

Usually do not track:

- PDFs
- large archives
- generated OCR dumps unless intentionally useful
- temporary extraction files

Before committing, check:

```text
git status --short --untracked-files=all
git diff --cached --name-only
```

The current setup already leaves the immunoglobulin PDF untracked.
