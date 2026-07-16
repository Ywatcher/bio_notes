# Claverie and Langman 1984 Model Note

Created: 2026-07-16

Paper: Jean-Michel Claverie and Rodney Langman, "Models for the rearrangements of immunoglobulin genes: a computer view" \cite{ClaverieLangman1984ComputerView}.

## Core Question

The paper asks whether observed immunoglobulin gene rearrangement patterns can be explained by a quantitative stochastic model. At the time, two broad explanations were being discussed:

1. A probabilistic model, where rearrangement is error-prone and allelic/isotype exclusion emerges from failed versus productive rearrangements.
2. A developmental hierarchy model, where some loci become recombinationally competent before others.

Claverie and Langman merge these into one stochastic model: apparent hierarchy can arise from biased probabilities.

## Minimal Model

The authors reduce rearrangement to three quantities:

```text
initiation efficiency:
    probability that a locus is chosen as the next target

fusion efficiency:
    probability that a rearrangement is productive rather than nonproductive

exit condition:
    rule for when rearrangement stops at a locus or in a cell
```

For light-chain rearrangement, they model the cell as moving through states defined by how many germ-line kappa and lambda alleles remain:

```text
state = (nK, nL)

nK = number of germ-line kappa alleles remaining
nL = number of germ-line lambda alleles remaining
```

The starting light-chain state is:

```text
(2, 2)
```

because the cell begins with two kappa alleles and two lambda alleles in germ-line configuration.

## Branching Ratio

A central parameter is the kappa/lambda branching ratio, written here as `b`.

```text
b = tendency to initiate kappa rearrangement relative to lambda rearrangement
```

If:

```text
b = 1
```

then kappa and lambda are treated symmetrically.

If:

```text
b >> 1
```

then kappa rearrangement tends to occur before lambda rearrangement.

The important point is that this is not a strict rule. It is a probability bias. Rare lambda-first or unusual configurations can still occur.

## Light-Chain Algorithm

A simplified version of the paper's light-chain model:

```text
initialize state = (2 germ-line kappa alleles, 2 germ-line lambda alleles)
initialize pK = probability that a kappa rearrangement is productive
initialize pL = probability that a lambda rearrangement is productive
initialize b = kappa/lambda branching ratio

while germ-line kappa or lambda alleles remain:

    choose locus to rearrange:
        probability(kappa) depends on nK, nL, and b
        probability(lambda) = 1 - probability(kappa)

    rearrange one germ-line allele at chosen locus

    if rearrangement is productive:
        express that light chain
        stop further light-chain rearrangement
        exit pathway

    if rearrangement is nonproductive:
        mark that allele as rearranged/non-germ-line
        continue if another germ-line allele remains

if no productive light chain is made:
    cell is predicted to be light-chain-null / unable to make full Ig
```

This is a stochastic state-transition model. The computer simulation calculates the probabilities of ending in different observed rearrangement patterns.

## Exit Condition

The paper tests two possibilities:

```text
stop signal model:
    productive fusion stops further rearrangement

no-stop model:
    rearrangement continues until no germ-line alleles remain
```

For light-chain loci, the experimental data fit the stop-signal model better. A productive kappa or lambda rearrangement appears to inhibit further light-chain rearrangement.

Algorithmically:

```text
if productive_light_chain:
    stop light-chain search
else:
    keep searching if substrates remain
```

## Best-Fit Parameters

For light chains, the best fit reported in the paper is approximately:

```text
pK = pL = 0.3
b = 17-22
```

Interpretation:

- Kappa and lambda have similar productive fusion probabilities.
- The main asymmetry is not that kappa joins are intrinsically more productive.
- The main asymmetry is that kappa is much more likely to be initiated before lambda.

The authors connect `p ~= 0.3` to the idea that random joining preserves the reading frame about one-third of the time.

## Heavy-Chain Rearrangement

The heavy-chain locus behaves differently in the data. More than 80% of murine Ig-positive cells show rearrangement of both heavy-chain alleles. This does not fit a simple model where a productive heavy-chain rearrangement immediately stops all further heavy-chain rearrangement.

The authors therefore suggest that the relevant stop signal may be the appearance of a functional immunoglobulin molecule, not merely a productive heavy-chain gene.

A simplified heavy-chain interpretation:

```text
heavy-chain rearrangement tends to occur before light-chain rearrangement
productive heavy-chain rearrangement alone may not immediately stop all rearrangement
complete functional immunoglobulin provides the effective stop signal
```

They estimate that a heavy-chain productive fusion probability around:

```text
pH ~= 0.1
```

would be plausible, because making a heavy-chain gene involves more than one joining step.

## Main Insight

The central insight is that hierarchy and probability are not opposites.

The observed order:

```text
heavy chain -> kappa light chain -> lambda light chain
```

can emerge from biased stochastic initiation probabilities rather than from an absolute deterministic program.

In algorithmic terms:

```text
strict hierarchy:
    always try H, then K, then L

Claverie-Langman stochastic hierarchy:
    H is much more likely before light chain
    K is much more likely before L
    rare exceptions remain possible
```

This explains why small experimental samples may look almost strictly hierarchical while still allowing unusual cells that violate a strict order.

## Relation to Selection

The 1984 model is not an antigen-selection model. The target of the modeled process is not a specific useful antibody against a pathogen. The target is simply a structurally productive immunoglobulin gene arrangement.

Algorithmically:

```text
generate rearrangement
if productive:
    stop / express
else:
    try another allele if possible
```

The model therefore describes early repertoire generation and exclusion, not later optimization against antigen.

## Predicted Null Cells

The model predicts cells that fail to make a functional light chain because all available rearrangement attempts are nonproductive. In the best-fit light-chain solution, the predicted fraction of null cells is around 17-24%.

The authors emphasize that experimental measurement of such null cells would provide an important test of the model.

## Computer View

The "computer view" is the representation of B-cell development as a state-space process:

```text
states:
    number and configuration of remaining germ-line alleles

transitions:
    probabilistic rearrangement events

transition weights:
    initiation probabilities and branching ratios

absorbing/exiting states:
    productive rearrangement and stop signal
    or failure after substrates are exhausted

model test:
    compare simulated final rearrangement patterns with observed data
```

The paper's contribution is not that B cells literally compute with symbols. It is that a biological recombination process can be formalized as a stochastic algorithm and quantitatively tested against experimental distributions.

## Compact Pseudocode

```text
for each developing B cell:

    perform heavy-chain rearrangement with high priority

    if heavy-chain process can support later Ig formation:
        enter light-chain rearrangement
    else:
        fail or remain null

    state = (2 kappa germ-line alleles, 2 lambda germ-line alleles)

    while light-chain germ-line alleles remain:

        choose kappa or lambda using branching ratio b

        attempt rearrangement on one germ-line allele

        if productive:
            express light chain
            stop light-chain rearrangement
            exit as Ig-positive cell

        else:
            remove that allele from germ-line pool
            continue

    if no productive light-chain rearrangement occurred:
        exit as null / non-Ig-producing cell
```

## Takeaway

Claverie and Langman use computation to show that biased stochastic rearrangement can reproduce key features of immunoglobulin gene rearrangement. Their model turns biological development into a probabilistic search process with state transitions, branching ratios, productive-fusion probabilities, and exit conditions.
