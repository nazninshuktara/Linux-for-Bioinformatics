# 8. Differential Expression Analysis — DESeq2
---

## 8.1 What Is Differential Expression Analysis?

It identifies genes whose expression differs significantly between conditions (e.g. Disease vs Healthy), classified as upregulated (higher in disease) or downregulated (lower in disease) — collectively, Differentially Expressed Genes (DEGs).

`DESeq2` handles normalization, biological variability, dispersion estimation, hypothesis testing, and multiple-testing correction. Basic workflow:

```bash
Gene-level count matrix + metadata  →  DESeqDataSet  →  DESeq()  →  results()  →  DEGs
```

> ⚠️ **Warning:** `DESeq2` needs count-like data from `tximport`, not `TPM`. Do not feed `TPM` values directly into `DESeq2`.

## 8.2 Preparing Metadata

```r
metadata$Condition <- factor(metadata$Condition)
metadata$Condition <- relevel(metadata$Condition, ref = "Healthy")   # Healthy = baseline
levels(metadata$Condition)     # should show: Healthy  disease
```

> **Note:** Setting Healthy as the reference level means `DESeq2` reports the disease effect relative to Healthy — this makes fold-change direction easy to interpret.

## 8.3 Building and Filtering the DESeq2 Object

```r
library(DESeq2)

dds <- DESeqDataSetFromTximport(
  txi = txi,
  colData = metadata,
  design = ~ Condition
)

# Remove genes with essentially no signal (example threshold, not a universal rule)
keep <- rowSums(counts(dds) >= 10) >= 3
dds <- dds[keep, ]
```

design = \~ Condition tells `DESeq2` to test whether expression differs according to Condition. When a genuine, appropriately-recorded confounder such as batch exists, extend the design (design = \~ Batch + Condition) — never add variables blindly.

## 8.4 Running DESeq2 and Extracting Results

```r
dds <- DESeq(dds)
res <- results(dds)
summary(res)
head(res)
```

| Column | Meaning |
| --- | --- |
| `baseMean` | average normalized expression across all samples |
| `log2FoldChange` | direction & magnitude of change; +2 ≈ 4× higher in disease, −2 ≈ ¼ of Healthy level |
| `pvalue` | raw statistical significance for one gene |
| `padj` | multiple-testing-adjusted p-value — use THIS to call significance, not raw `pvalue` |

> **Note:** With thousands of genes tested at once, some will look "significant" by chance alone — `padj` (Benjamini-Hochberg by default) controls this false-discovery problem.

## 8.5 Selecting Significant DEGs

```r
sig <- res[!is.na(res$padj) & res$padj < 0.05 & abs(res$log2FoldChange) >= 1, ]
up   <- sig[sig$log2FoldChange >=  1, ]
down <- sig[sig$log2FoldChange <= -1, ]
nrow(sig); nrow(up); nrow(down)

sig_sorted <- sig[order(sig$padj), ]     # strongest evidence first

write.csv(as.data.frame(res),  "results/DESeq2_all_results.csv")
write.csv(as.data.frame(sig),  "results/DESeq2_significant_DEGs.csv")
write.csv(as.data.frame(up),   "results/upregulated_genes.csv")
write.csv(as.data.frame(down), "results/downregulated_genes.csv")
```

`padj` < 0.05 together with |log2FC| ≥ 1 is a common, but not universal, starting threshold — choose and justify thresholds for your own study.

## 8.6 Interpretation Caveats

- Differential expression ≠ causation. "Gene X is significantly upregulated in disease vs Healthy" is defensible; "Gene X causes disease" is not.

- Bulk RNA-seq measures a mixture of cell types per sample — some apparent expression changes may reflect shifts in cell-type composition rather than per-cell regulatory changes.

- Batch effects: if Condition happens to align with Batch (e.g. all disease samples sequenced in one batch), the two are confounded and cannot be cleanly separated. Fix this at experimental-design stage where possible, and model it (\~ Batch + Condition) only when metadata genuinely support it.

- Never remove a sample just because it looks inconvenient in PCA — investigate metadata, sequencing quality, library size, and possible sample-identity mix-ups first.

