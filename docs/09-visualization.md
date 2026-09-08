# 9. Visualization: PCA, Heatmap, Volcano, MA Plot
---

A table of 15,000 genes is unreadable; figures immediately show whether samples separate by condition, which genes moved most, and whether there are outliers.

| Figure | Main question it answers |
| --- | --- |
| `PCA` | Do samples separate by condition at the whole-transcriptome level? |
| `Heatmap` | Do the top DEGs show a consistent pattern across samples? |
| `Volcano plot` | Which genes are both large-effect AND statistically significant? |
| `MA plot` | How are fold-changes distributed across expression levels? |

## 9.1 PCA (Principal Component Analysis)

```r
library(ggplot2)
vsd <- vst(dds, blind = FALSE)     # variance-stabilizing transform, used for exploratory plots
pca_data <- plotPCA(vsd, intgroup = "Condition", returnData = TRUE)
percent_var <- round(100 * attr(pca_data, "percentVar"))

ggplot(pca_data, aes(x = PC1, y = PC2, color = Condition, label = name)) +
  geom_point(size = 4) + geom_text(vjust = -0.8) +
  xlab(paste0("PC1: ", percent_var[1], "% variance")) +
  ylab(paste0("PC2: ", percent_var[2], "% variance")) +
  theme_classic()

ggsave("figures/PCA_disease_vs_Healthy.png", width = 7, height = 5, dpi = 300)
```

If disease and Healthy samples cluster separately, condition explains a large share of the variance. If they don't separate, other factors (batch, sex, age, cell composition, technical variation) may dominate — that alone doesn't mean the analysis failed.

## 9.2 Heatmap of Top DEGs

```r
library(pheatmap)
top_genes <- head(rownames(sig_sorted), 30)
heatmap_data <- assay(vsd)[top_genes, ]

pheatmap(heatmap_data,
  annotation_col = as.data.frame(colData(dds)[, "Condition", drop = FALSE]),
  scale = "row", cluster_rows = TRUE, cluster_cols = TRUE)
```

scale = "row" standardizes each gene across samples, making relative expression patterns easier to see. Samples from the same condition clustering together is encouraging, but clustering alone doesn't prove biological correctness.

## 9.3 Volcano Plot

```r
volcano_data <- as.data.frame(res)
volcano_data$gene <- rownames(volcano_data)
volcano_data <- volcano_data[!is.na(volcano_data$padj) & !is.na(volcano_data$log2FoldChange), ]

volcano_data$group <- "Not significant"
volcano_data$group[volcano_data$padj < 0.05 & volcano_data$log2FoldChange >=  1] <- "Upregulated"
volcano_data$group[volcano_data$padj < 0.05 & volcano_data$log2FoldChange <= -1] <- "Downregulated"
volcano_data$neg_log10_padj <- -log10(volcano_data$padj)

ggplot(volcano_data, aes(x = log2FoldChange, y = neg_log10_padj)) +
  geom_point(aes(color = group), alpha = 0.7) +
  geom_vline(xintercept = c(-1, 1), linetype = "dashed") +
  geom_hline(yintercept = -log10(0.05), linetype = "dashed") +
  theme_classic()
```

**Large fold-change does not necessarily mean statistically significant, and statistical significance does not necessarily mean biologically important — the volcano plot lets you weigh both at once.**

## 9.4 MA Plot

```bash
plotMA(res, ylim = c(-5, 5))
```

X-axis = average expression, Y-axis = log2 fold change — useful for spotting whether fold-changes behave differently at low vs high expression levels.
