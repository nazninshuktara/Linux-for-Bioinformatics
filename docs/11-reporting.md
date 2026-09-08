# 11. Writing Up the Project
---

## 11.1 Analysis Order (avoid confirmation bias)

Do quality control and technical checks BEFORE looking for expected biology. Correct order:

```bash
QC 
 → Metadata 
   → Expression data 
     → DESeq2 
       → PCA 
         → DEGs 
           → Enrichment 
             → Biological interpretation
```

> ⚠️ **Warning:** Wrong order: starting from "what pathways does the literature expect" and then searching your results for them — this creates confirmation bias.

## 11.2 Methods Section — What to Include

- Data source (e.g. NCBI GEO accession numbers)

- Sample groups and sizes (Disease vs Healthy, n per group)

- Quantification tool (Salmon) and reference transcriptome used

- Transcript-to-gene summarization (`tximport`)

- Differential expression tool and design formula (`DESeq2`, design = \~ Condition)

- Significance thresholds used (`padj`, |log2FC|)

- Visualization methods (PCA, heatmap, volcano, MA)

- Functional analysis tools (GO, KEGG, Reactome, GSEA) and databases

> ⚠️ **Warning:** Only state tools, reference genomes, versions, and thresholds you actually used — never copy a template value you haven't verified.


## 11.3 Results — a Simple Writing Formula

**Finding → Evidence → Interpretation**

Example: "`DESeq2` identified X significant DEGs (Y up, Z down). PCA showed [actual pattern]. Functional enrichment highlighted [actual themes], with KEGG, Reactome, and GSEA showing [actual convergent/divergent findings]."


## 11.4 Limitations to Discuss

- Dataset heterogeneity across studies/protocols/platforms if combining multiple GEO datasets

- Batch effects and possible confounding with condition

- Limited sample size / biological replicates

- Bulk RNA-seq cannot separate expression change from cell-composition change

- Missing or inconsistent clinical metadata in public datasets

- Association ≠ causation — differential expression is observational

> ⚠️ **Warning:** Never simply concatenate expression matrices from unrelated studies and run `DESeq2` without modeling study/batch effects and checking cross-study compatibility.


## 11.5 Suggested Project README Sections

- Overview / Research Question

- Dataset

- Study Design

- Workflow (diagram or bullet list)

- Software & Tools (with an exported `environment.yml`)

- Differential Expression Analysis

- Functional Enrichment

- Results (key figures)

- Reproducibility

- Limitations
