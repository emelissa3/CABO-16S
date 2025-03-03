![Alt text](CABO.png)\
\
CABO-16S -- A Combined Archaea, Bacteria, Organelle 16S database for amplicon analysis of prokaryotes and eukaryotes in environmental samples

[updated 03 March 2025]

## Overview

The widespread adoption of 16S rRNA amplicon sequencing has greatly advanced surveys of microbial diversity. A key step in many such analyses includes taxonomic annotation of the obtained sequences. For this annotation step, multiple databases for 16S rRNA exist, including [SILVA](https://www.arb-silva.de/), [GreenGenes](https://greengenes2.ucsd.edu/), and [PR2](https://pr2-database.org/).

Due to its breadth and consistent maintenance, SILVA is one of the most popular databses. However, modern versions of SILVA are poorly compatible with plastid sequences, and some information may be lacking for particular groups of environmental interest (e.g., uncultured symbiotic deep-sea sulfate-reducing bacteria).

We joined existing databases to allow combined annotation of prokaryotic and plastidal 16S rRNA sequences. This new database combines the entire set of prokaryotic sequences from SILVA-138.2 with plastids from the PR2 database. Since this operation includes a merge step, we also merged previously unpublished lab sequences to produce a new training set.

### Quickstart

Ready to try CABO-16S? If you use DADA2 in R, you can simply download `CABO-16S.Rdata` [from FigShare](https://doi.org/10.6084/m9.figshare.27288090.v2) and use it directly like

```         
load("CABO-16S.RData")  # variable named trainingSet
DECIPHER::IdTaxa(test=asv_seqs, trainingSet=trainingSet, strand="top", ...)
```

If you use QIIME2, we also provide a fasta `CABO-16S.fasta.gz` [on FigShare](https://doi.org/10.6084/m9.figshare.27288090.v2) that you can use.

### How to reproduce, update, or customize

The steps to create or update CABO-16S are spread across 2 scripts, with two more scripts included as a recommended start to benchmarking whether the merged database is actually an improvement.

Constructing a database:

-   [combining_CABO-16S.Rmd](https://github.com/emelissa3/CABO-16S/blob/main/combining_CABO-16S.Rmd)

    -   This file describes the downloading, cleaning, and merging of the 'main' 16S rRNA database with organellar 16S rRNA sequences from PR2. Note that we use SILVA 138.2, however, any database could be used (GTDB, GreenGenes2, etc.) albeit with some different cleaning steps. If users have FASTA-formatted custom sequences to be included, those may be similarly merged here.

-   [training_CABO-16S.Rmd](https://github.com/emelissa3/CABO-16S/blob/main/training_CABO-16S.Rmd).

    -   This script trains a classifier off of the combined FASTA with [IDTXA documentation](http://www2.decipher.codes/Documentation/Documentation-ClassifySequences.html). This script is heavily based on the IDTAXA package's recommendations as we find it to work quite well. [FigShare](https://doi.org/10.6084/m9.figshare.27288090.v2).

Benchmarking it:

-   [dada2_sets.Rmd](https://github.com/emelissa3/CABO-16S/blob/main/dada2_sets.Rmd)

    -   This RMarkdown describes a minimal set of approximately 100 publicly available V4V5 samples from a diversity of environments.

-   [benchmarking_CABO-16S.Rmd](https://github.com/emelissa3/CABO-16S/blob/main/benchmarking_CABO-16S.Rmd)

    -   This script applies the database produced by the `training_CABO-16S.Rmd` script to the ASVs analyzed in `dada2_sets.Rmd` and additionally contains the code to reproduce the figures presented in the manuscript.

## Notes on a successful merge

Briefly, the SILVA 138.2 fasta was obtained by dropping the eukaryotic (18S rRNA) sequences and otherwise left intact. CABO-16S is the result of merging this prokaryotic-only SILVA 138.2 with plastids from the [PR2](https://pr2-database.org/) database and a set of unpublished sequences.

### Recommended benchmarking data

The data are subsets of the following sets available on NCBI:

-   Methane seep - from [Marlow et al. 2021](https://www.pnas.org/doi/10.1073/pnas.2006857118) at [PRJNA648152](https://www.ncbi.nlm.nih.gov/sra/?term=PRJNA648152)

-   Hydrothermal vent - from [Speth et al. 2022](https://www.nature.com/articles/s41396-022-01222-x) at [PRJNA713414](https://www.ncbi.nlm.nih.gov/bioproject/713414)

-   Mono Lake water - from [Phillips et al 2021](https://onlinelibrary.wiley.com/doi/10.1111/gbi.12437) at [PRJNA702881](https://www.ncbi.nlm.nih.gov/bioproject/702881)

-   Seagrass - from [Kardish et al. 2023](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9985655/) at [PRJNA731931](https://www.ncbi.nlm.nih.gov/bioproject/731931)

-   Soil - from [Porter et al](%5Bhttps://www.nature.com/articles/s41598-023-30732-7#Sec9)](<https://www.nature.com/articles/s41598-023-30732-7#Sec9>)) at [PRJNA565010](%5Bhttps://www.ncbi.anlm.nih.gov/bioproject/565010)](<https://www.ncbi.anlm.nih.gov/bioproject/565010>))

-   Fouhy et al (mock communities) - from [Fouhy et al 2016](https://www.sciencedirect.com/science/article/pii/S200103702030516X) at [PRJNA315115](https://www.ncbi.nlm.nih.gov/bioproject/315115) *single-end reads*

-   [Kozich et al 2013](https://journals.asm.org/doi/10.1128/aem.01043-13) are not on NCBI but the specified FASTQ's were found from the [mothur website here](https://mothur.org/MiSeqDevelopmentData/)

\
\
Eryn M. Eitel, Daniel Utter, Stephanie A. Connon, Victoria J. Orphan, Ranjani Murali\
\
Abstract\
Identification of both prokaryotic and eukaryotic microorganisms in environmental samples is currently challenged by either the burden of additional sequencing required to obtain both 16S and 18S rRNA sequences or the introduction of multiple biases induced by the use of "universal" primers. Organellar 16S rRNA sequences are automatically amplified and sequenced along with prokaryote 16S rRNA, and may provide an alternative method to identify eukaryotic microorganisms. CABO-16S combines bacterial and archaeal sequences from the SILVA database with 16S rRNA sequences of plastids and other organelles from the PR2 database to enable identification of all 16S rRNA sequences. Comparison of CABO-16S with SILVA 138.2 results in equivalent taxonomic classification of mock communities and increased classification of diverse environmental samples. In particular, identification of phototrophic eukaryotes in shallow seagrass environments, marine waters, and lake waters was increased. The CABO-16S framework allows users to add custom sequences for further classification of underrepresented clades and can be easily updated with future releases of reference databases. Addition of sequences obtained from Sanger sequencing of methane seep sediments and curated sequences of the polyphyletic SEEP-SRB1 clade resulted in differentiation of syntrophic and non-syntrophic SEEP-SRB1 in hydrothermal vent sediments. Such additions may simplify analysis of communities contributing to the anaerobic oxidation of methane, and highlight the potential benefit of amending existing training sets with curated sequences when studying extreme or unique environments underrepresented in existing databases.

