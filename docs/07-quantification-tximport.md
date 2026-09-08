# 7. RNA-seq Quantification: quant.sf & tximport
---

## 7.1 What is quant.sf?

`quant.sf` is Salmon's transcript-quantification output, one file per sample:

```bash
Name             Length   EffectiveLength   TPM    NumReads
TX001            1500     1350              12.5   245.3
TX002            2200     2050              25.7   510.8
```

| Column | Meaning |
| --- | --- |
| `Name` | Transcript identifier (e.g. Ensembl ENST... or another reference-specific ID) |
| `Length` | Raw transcript length |
| `EffectiveLength` | Length adjusted for fragment-length / sequencing effects (Salmon-estimated, not calculated manually) |
| `TPM` | Transcripts Per Million — normalized abundance, good for visualization/comparison, not fed directly into `DESeq2` |
| `NumReads` | Estimated reads/fragments assigned to the transcript — can be fractional, because Salmon probabilistically assigns ambiguous reads across transcripts |

> **Note:** Fractional `NumReads` (e.g. 245.3) is normal — it reflects Salmon splitting reads across transcripts they could plausibly have come from.

## 7.2 Transcript-Level vs Gene-Level

Salmon quantifies at the transcript level (e.g. TP53-201, TP53-202, TP53-203), but differential-expression analysis is usually done at the gene level (TP53). Summarizing transcript-level counts into gene-level counts requires a transcript-to-gene (`tx2gene`) mapping.

```bash
TXNAME        GENEID
ENST000001    ENSG000001
ENST000002    ENSG000001    # two transcripts, same gene
ENST000003    ENSG000002
```

> ⚠️ **Warning:** The `tx2gene` mapping must correspond to the exact reference transcriptome Salmon used — don't reuse a random annotation file just because the ID format looks similar.

## 7.3 tximport

`tximport` is an R/Bioconductor package that imports multiple `quant.sf` files and produces gene-level abundance and count matrices ready for `DESeq2`.

```r
library(tximport)

files <- c(
  Sample01 = "data/quant/Sample01/quant.sf",
  Sample02 = "data/quant/Sample02/quant.sf",
  Sample03 = "data/quant/Sample03/quant.sf"
)

tx2gene <- read.csv("data/metadata/tx2gene.csv")

txi <- tximport(files, type = "salmon", tx2gene = tx2gene)

dim(txi$counts)          # rows = genes, columns = samples
head(txi$counts)
```

## 7.4 Matching Samples to Metadata (Critical QC Step)

```r
metadata <- read.csv("data/metadata/metadata.csv", row.names = 1)

all(colnames(txi$counts) %in% rownames(metadata))   # every sample has metadata?

# Re-order metadata to match the expression matrix exactly:
metadata <- metadata[colnames(txi$counts), ]
all(colnames(txi$counts) == rownames(metadata))     # must be TRUE before continuing

write.csv(txi$counts, "results/gene_counts_tximport.csv")
```

> ⚠️ **Warning:** If this equality check is FALSE, stop and fix the sample matching before running `DESeq2` — a silent mismatch here invalidates the entire downstream analysis.
