# 10. Functional & Pathway Enrichment
---

Once you have DEGs, the next question moves from "which genes changed" to "what biological functions/pathways are affected".

## 10.1 Gene Ontology (GO)

GO describes gene function in three categories:

| Category | Question | Examples |
| --- | --- | --- |
| `Biological Process (BP)` | What process does the gene participate in? | immune response, apoptosis, cell cycle |
| `Molecular Function (MF)` | What does it do at the molecular level? | DNA binding, kinase activity |
| `Cellular Component (CC)` | Where does it act? | nucleus, mitochondrion, plasma membrane |

```r
library(clusterProfiler); library(org.Hs.eg.db)

# Gene IDs must be checked first — GO/KEGG enrichment commonly needs Entrez IDs
up_conversion <- bitr(rownames(up), fromType = "ENSEMBL", toType = "ENTREZID", OrgDb = org.Hs.eg.db)

ego_up <- enrichGO(
  gene = up_conversion$ENTREZID, OrgDb = org.Hs.eg.db, keyType = "ENTREZID",
  ont = "BP", pAdjustMethod = "BH", pvalueCutoff = 0.05, qvalueCutoff = 0.05, readable = TRUE)

dotplot(ego_up, showCategory = 20)
write.csv(as.data.frame(ego_up), "results/GO_BP_upregulated.csv", row.names = FALSE)
```

Repeat separately for the downregulated set — analyzing up/down together can hide the direction of the biological signal.

> ⚠️ **Warning:** Gene-ID mismatches are a very common source of failed/empty enrichment results. Always check head(rownames(sig)) first to confirm whether you have Ensembl IDs, gene symbols, or Entrez IDs, and strip Ensembl version suffixes (e.g. .17) with sub("\\..`*`$", "", ids) when present.

## 10.2 KEGG Pathway Enrichment

KEGG organizes genes into biological/signalling pathways (GO = functions, KEGG = pathways).

```r
ekegg_up <- enrichKEGG(
  gene = up_conversion$ENTREZID, organism = "hsa",
  pAdjustMethod = "BH", pvalueCutoff = 0.05, qvalueCutoff = 0.05)

dotplot(ekegg_up, showCategory = 20)
write.csv(as.data.frame(ekegg_up), "results/KEGG_upregulated.csv", row.names = FALSE)
```

> **Note:** organism = "hsa" is Homo sapiens; use the matching code for other organisms (e.g. "mmu" for mouse) and the corresponding OrgDb (`org.Mm.eg.db`).

## 10.3 Reactome

Reactome is a curated pathway database offering more granular, hierarchical detail than KEGG (e.g. Immune System → Innate Immune System → Cytokine Signaling → specific events).

```r
library(ReactomePA)
reactome_up <- enrichPathway(
  gene = up_conversion$ENTREZID, organism = "human",
  pAdjustMethod = "BH", pvalueCutoff = 0.05, qvalueCutoff = 0.05, readable = TRUE)
dotplot(reactome_up, showCategory = 20)
```

## 10.4 ORA vs GSEA

|  | ORA (Over-Representation Analysis) | GSEA (Gene Set Enrichment Analysis) |
| --- | --- | --- |
| `Input` | A selected gene list (e.g. significant DEGs) | ALL genes, ranked by a statistic |
| `Requires a DEG cutoff?` | Yes | No |
| `Can miss subtle, coordinated changes?` | Yes | Designed to catch these |
| `Typical functions used` | `enrichGO` / `enrichKEGG` / `enrichPathway` | `gseGO` / `gseKEGG` |

Ranking genes for GSEA (using `DESeq2`'s stat column, which preserves direction):

```r
res_df <- as.data.frame(res)
res_df <- res_df[!is.na(res_df$stat), ]
gene_list <- res_df$stat
names(gene_list) <- rownames(res_df)
gene_list <- sort(gene_list, decreasing = TRUE)
anyDuplicated(names(gene_list))     # check for duplicate IDs before running GSEA

gsea_kegg <- gseKEGG(geneList = gene_list, organism = "hsa", pAdjustMethod = "BH", pvalueCutoff = 0.05)
dotplot(gsea_kegg, showCategory = 20)
gseaplot2(gsea_kegg, geneSetID = 1)          # classic enrichment-curve plot for one pathway
```

| GSEA term | Meaning |
| --- | --- |
| `NES` | Normalized Enrichment Score. NES > 0 → gene set skewed toward the top (positive/first-listed condition) of the ranking; NES < 0 → skewed toward the bottom. |
| `FDR / p.adjust` | Multiple-testing-adjusted significance — use this, not the raw p-value. |
| `core_enrichment / leading edge` | The specific genes actually driving a pathway's enrichment signal. |

> ⚠️ **Warning:** The gene-ID system in your ranked list must match the ID system of the pathway database, or GSEA will fail or give poor/empty results.

## 10.5 Integrating GO + KEGG + Reactome + GSEA

Don't interpret each result table in isolation — look for a converging biological theme across independent methods, e.g.:

| Biological theme | GO | KEGG | Reactome | GSEA |
| --- | --- | --- | --- | --- |
| `Immune response` | ✓ | ✓ | ✓ | ✓ |
| `Cytokine signaling` | ✓ | ✓ | ✓ | ✓ |
| `Antiviral response` | ✓ | — | ✓ | ✓ |

- Convergent evidence across methods increases confidence in a biological interpretation, but does not by itself prove causation or mechanism.

- Enrichment terms are often redundant (many share the same underlying genes) — summarize by biological theme rather than counting every significant row.

- Prefer cautious language: "was significantly enriched" / "was implicated" rather than "activates" or "causes".

- Enrichment of a pathway does not mean every gene in it moved in the same direction — check the core-enrichment genes for the real drivers.
