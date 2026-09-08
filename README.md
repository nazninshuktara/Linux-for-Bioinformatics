# 🐧 Linux-for-Bioinformatics
Self-study notes on Linux, Bash scripting, and R/Bioconductor tools for bioinformatics.
> Prepared by **Naznin**.
---

## 📖 Overview
This repository serves as a personal knowledge base and practical guide for using Linux CLI, Bash automation, and R/Bioconductor environments for bioinformatics data analysis (e.g., RNA-seq, sequence analysis).
---

## 🛠️ Topics Covered

### 🔬 1. Bioinformatics introduction
- Bioinformatics and its importance
- Why Linux for Bioinformatics

### 🐧 2. Linux fundamentals
- Directory navigation (`pwd`, `ls`, `cd`, `~`, `..`)
- File manipulation (`mkdir`, `cp`, `mv`, `rm`, `touch`)
- Listing options (`ls -lhS`, `ls -la`)
- File viewing & processing (`cat`, `head`, `tail`, `grep`, `uniq -c`, `awk`, `sed`)
- File permissions & environment variables (`chmod`,`|`, `>`, `>>`, `tee -a`, `PATH`)

### 🧬 3. Biological File Formats & Quality Control
- FASTA, FASTQ structure, and Phred Quality Scores
- Direct stream processing of `.gz` files using `zcat`, `zless`, and `zgrep`
- Single-sample assessment using `FastQC` and aggregate reporting with `MultiQC`

### 📜 4. Bash Scripting and project organization for Bioinformatics
- Shell script fundamentals (`#!/bin/bash`)
- Variables, positional parameters (`$1`, `$2`), and script arguments (`$#`, `"$@"`)
- Loops and conditionals for batch processing (`for` loops, `if/else` conditionals)
- Writing automated workflows for raw sequencing data (.fastq / .fasta)
- Project Organization & Sample Naming

### 📦 5. Environment Management (Conda/Mamba)
- Creating, activating, deactivating, and managing Conda/Mamba environments.
- Managing channels (`bioconda`, `conda-forge`) to install tools (`salmon`, `fastqc`, and `multiqc`)
- Exporting (`environment.yml`) and recreating identical project environments

### 📊 6. RNA-seq Quantification & Processing (Salmon & tximport)
- Understanding `quant.sf` outputs, including `Length`, `EffectiveLength`, `TPM`, and fractional `NumReads`
- Mapping transcripts to genes (`tx2gene`) and loading counts via `tximport`
- Validation and strict alignment between expression count columns and metadata sample IDs

### 🔬 6. Differential Expression Analysis (DESeq2)
- Baseline setting (`relevel`), confounding modeling (`design = ~ Condition`), and count filtering
- Interpreting `baseMean`, `log2FoldChange`, `pvalue`, and `padj` (Benjamini-Hochberg adjustment)
- Extracting significantly upregulated and downregulated gene sets

### 🎨 7. Visualization & Pathway Enrichment
- Variance Stabilizing Transformation (`vst`), Principal Component Analysis (PCA), heatmaps (`pheatmap`), Volcano plots, and MA plots
- Over-Representation Analysis (ORA) across Gene Ontology (BP, MF, CC), KEGG, and Reactome pathways
- Ranked-list testing using NES (Normalized Enrichment Scores) without arbitrary cutoff thresholds
- Writing Up the Project

---

## 📁 Repository Structure
```
bioinformatics-linux-notes/
├── README.md       
├── LICENSE (MIT)
├── .gitignore
└── docs/
    ├── 01-introduction.md
    ├── 02-linux-fundamentals.md
    ├── 03-file-formats.md
    ├── 04-bash-scripting.md
    ├── 05-project-organization.md
    ├── 06-conda-environments.md
    ├── 07-quantification-tximport.md
    ├── 08-deseq2.md
    ├── 09-visualization.md
    ├── 10-enrichment-analysis.md
    ├── 11-reporting.md
    ├── appendix-a-cheatsheet.md
    └── appendix-b-interview-qa.md
```

## 📌 How to use this repo

Each section in `docs/` can be read on its own, or in order as a course. `Appendix A` is a
one-page command cheat sheet; `Appendix B` collects revision questions for every
topic covered.

- [1. Introduction to Bioinformatics](docs/01-introduction.md)
- [2. Linux Fundamentals](docs/02-linux-fundamentals.md)
- [3. Biological File Formats](docs/03-file-formats.md)
- [4. Bash Scripting](docs/04-bash-scripting.md)
- [5. Project Organization & Sample Naming](docs/05-project-organization.md)
- [6. Environment Management — Conda / Mamba](docs/06-conda-environments.md)
- [7. RNA-seq Quantification: quant.sf & tximport](docs/07-quantification-tximport.md)
- [8. Differential Expression Analysis — DESeq2](docs/08-deseq2.md)
- [9. Visualization: PCA, Heatmap, Volcano, MA Plot](docs/09-visualization.md)
- [10. Functional & Pathway Enrichment](docs/10-enrichment-analysis.md)
- [11. Writing Up the Project](docs/11-reporting.md)
- [Appendix A — Master Command Cheat Sheet](docs/appendix-a-cheatsheet.md)
- [Appendix B — Revision Questions](docs/appendix-b-interview-qa.md)

