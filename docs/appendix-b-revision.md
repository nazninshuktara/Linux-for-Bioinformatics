# Appendix B — Revision Questions
---

## B.1 Linux & Bash

- What does `pwd` / ls / cd do? — Show current directory / list contents / change directory.

- What is a pipe (|)? — It sends the output of one command as the input of the next.

- What does `grep` -r do? — Searches recursively through files in the current directory and its subdirectories.

- What does cut -f 1 do? — Extracts field/column 1 from delimited text.

- What does `uniq` -c do? — Counts occurrences of each unique (consecutive) line.

- What is the difference between `$1` and `$#`? — `$1` is the first argument; `$#` is the total number of arguments supplied.

- Why use set -euo pipefail? — It makes Bash scripts safer by stopping on command errors, treating undefined variables as errors, and catching failures inside pipelines.

- What does `${var%pattern}` do? — Removes the matching pattern from the end of a variable's value — useful for deriving a sample name from a filename.

## B.2 FASTQ & Sequencing

- FASTA vs FASTQ? — FASTA stores sequence only; FASTQ stores sequence plus a per-base quality string, in 4-line records.

- What is Q30? — A Phred quality score corresponding to a 0.1% error probability, i.e. ≈99.9% base-call accuracy.

- What are R1 and R2? — The two reads generated from opposite ends of the same fragment in paired-end sequencing; they belong to the same biological sample and must not be mixed across samples.


## B.3 Quantification

- What is `quant.sf`? — Salmon's transcript-level quantification output, containing Length, EffectiveLength, `TPM`, and `NumReads` per transcript.

- Why can `NumReads` be fractional? — Because Salmon probabilistically assigns ambiguous reads across the transcripts they could have originated from.

- Why use `tximport` instead of `quant.sf` directly in `DESeq2`? — `DESeq2` needs a gene-level count matrix (genes × samples); `tximport` summarizes transcript-level Salmon output to that format using a `tx2gene` mapping.


## B.4 DESeq2

- What is differential expression analysis? — Identifying genes whose expression levels differ significantly between conditions.

- What is `log2FoldChange`? — The magnitude and direction of expression change on a log2 scale, relative to the reference level.

- What does `padj` represent, and why use it instead of the raw p-value? — The multiple-testing-adjusted p-value; with thousands of genes tested simultaneously, raw p-values alone would produce many false positives by chance.

- Why set Healthy as the reference level? — So fold changes are interpreted consistently as "COVID relative to Healthy".

- What is a DEG? — A gene showing statistically significant evidence of altered expression between the compared conditions.


## B.5 Visualization

- Why run `vst`() before PCA? — It stabilizes variance across the range of expression levels, making the data more suitable for exploratory visualization.

- What does PCA separation indicate? — Different groups have different global expression profiles — though the separation could also reflect batch or other technical/biological factors, not condition alone.

- What does a volcano plot show? — log2 fold-change (x-axis) against statistical significance, typically -log10(`padj`) (y-axis).

- What is an outlier, and how should it be handled? — A sample whose expression profile differs substantially from others; it should be investigated (metadata, QC, batch, identity), not removed automatically.


## B.6 GO / KEGG / Reactome / GSEA

- What are the three GO categories? — Biological Process, Molecular Function, Cellular Component.

- GO vs KEGG? — GO gives standardized functional annotations; KEGG organizes genes into biological/signaling pathways.

- What is ORA? — Over-Representation Analysis: tests whether predefined categories are overrepresented in a selected gene list (e.g. significant DEGs).

- What is GSEA, and how does it differ from ORA? — Gene Set Enrichment Analysis tests whether a gene set is concentrated toward the top or bottom of a ranked list of ALL genes, without requiring a DEG cutoff first — so it can detect coordinated changes that individually fall short of significance.

- What is NES? — Normalized Enrichment Score; its sign indicates the direction of enrichment and its magnitude the strength, after normalizing for gene-set size.

- Why analyze upregulated and downregulated genes separately? — They may represent different, even opposite, biological processes; combining them can obscure the direction of the biological signal.

- Does pathway enrichment prove pathway activation? — No — it indicates statistical overrepresentation or coordinated ranking of pathway-associated genes, not proof of a functional mechanism.

- Why is gene-ID consistency important for enrichment analysis? — The identifiers in the gene list must match those used by the annotation/pathway database, or genes will fail to map and results will be incomplete or wrong.
