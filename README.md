# 🐧 Linux-for-Bioinformatics

Self-study notes on Linux, Bash scripting, and R/Bioconductor tools for bioinformatics.

> Prepared by **Naznin**.

---

## 📖 Overview

This repository serves as a personal knowledge base and practical guide for using Linux CLI, Bash automation, and R/Bioconductor environments for bioinformatics data analysis (e.g., RNA-seq, sequence analysis).

---

## 🛠️ Topics Covered

### 🔬 1. Bioinformatics Introduction
- Bioinformatics and its importance
- Why Linux for Bioinformatics

### 🐧 2. Linux Fundamentals
- Directory navigation (`pwd`, `ls`, `cd`, `~`, `..`)
- File manipulation (`mkdir`, `cp`, `mv`, `rm`, `touch`)
- Listing options (`ls -lhS`, `ls -la`)
- File viewing & processing (`cat`, `head`, `tail`, `grep`, `uniq -c`, `awk`, `sed`)
- File permissions & environment variables (`chmod`, `|`, `>`, `>>`, `tee -a`, `PATH`)

### 🧬 3. Biological File Formats & Quality Control
- FASTA, FASTQ structure, and Phred Quality Scores
- Direct stream processing of `.gz` files using `zcat`, `zless`, and `zgrep`
- Single-sample assessment using `FastQC` and aggregate reporting with `MultiQC`

### 📜 4. Bash Scripting & Project Organization
- Shell script fundamentals (`#!/bin/bash`)
- Variables, positional parameters (`$1`, `$2`), and script arguments (`$#`, `"$@"`)
- Loops and conditionals for batch processing (`for` loops, `if/else` conditionals)
- Writing automated workflows for raw sequencing data (.fastq / .fasta)
- Project Organization & Sample Naming

### 📦 5. Environment Management (Conda/Mamba)
- Creating, activating, deactivating, and managing Conda/Mamba environments
- Managing channels (`bioconda`, `conda-forge`) to install tools (`salmon`, `fastqc`, `multiqc`)
- Exporting (`environment.yml`) and recreating identical project environments

### 📊 6. RNA-seq Quantification & Processing (Salmon & tximport)
- Understanding `quant.sf` outputs, including `Length`, `EffectiveLength`, `TPM`, and fractional `NumReads`
- Mapping transcripts to genes (`tx2gene`) and loading counts via `tximport`
- Validation and strict alignment between expression count columns and metadata sample IDs

### 🔬 7. Differential Expression Analysis (DESeq2)
- Baseline setting (`relevel`), confounding modeling (`design = ~ Condition`), and count filtering
- Interpreting `baseMean`, `log2FoldChange`, `pvalue`, and `padj` (Benjamini-Hochberg adjustment)
- Extracting significantly upregulated and downregulated gene sets

### 🎨 8. Visualization & Pathway Enrichment
- Variance Stabilizing Transformation (`vst`), Principal Component Analysis (PCA), heatmaps (`pheatmap`), Volcano plots, and MA plots
- Over-Representation Analysis (ORA) across Gene Ontology (BP, MF, CC), KEGG, and Reactome pathways
- Ranked-list testing using NES (Normalized Enrichment Scores) without arbitrary cutoff thresholds
- Writing Up the Project

---

## 📁 Repository Structure
```text
Linux-for-Bioinformatics/
├── README.md                       # Main repo overview & guide
├── LICENSE                         # MIT license
├── .gitignore                      # Git ignore file
└── docs/
    ├── 01-introduction.md              # Intro to Bioinformatics & Linux basics
    ├── 02-linux-fundamentals.md        # Linux CLI navigation & text tools
    ├── 03-file-formats.md              # FASTA/FASTQ formats, Phred scores & QC
    ├── 04-bash-scripting.md            # Shell scripts, loops & QC automation
    ├── 05-project-organization.md      # Directory setup & sample naming rules
    ├── 06-conda-environments.md        # Conda package & environment setup
    ├── 07-quantification-tximport.md   # Salmon quant.sf & tximport mapping
    ├── 08-deseq2.md                    # DESeq2 analysis & DEG identification
    ├── 09-visualization.md             # PCA, Heatmap, Volcano & MA plots
    ├── 10-enrichment-analysis.md       # GO, KEGG, Reactome & GSEA analysis
    ├── 11-reporting.md                 # Methods, results & project write-up
    ├── appendix-a-cheatsheet.md        # Master command cheat sheet
    └── appendix-b-interview-qa.md      # Revision & interview Q&A guide
```

## 📌 How to use this repo

Each section in `docs/` can be read on its own, or in order as a course. `Appendix A` is a
one-page command cheat sheet; `Appendix B` collects revision questions for every
topic covered.

- [1. Introduction to Bioinformatics](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/01-introduction.md)
- [2. Linux Fundamentals](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/02-linux-fundamentals.md)
- [3. Biological File Formats](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/03-file-formats.md)
- [4. Bash Scripting](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/04-bash-scripting.md)
- [5. Project Organization & Sample Naming](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/05-project-organization.md)
- [6. Environment Management — Conda / Mamba](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/06-conda-environments.md)
- [7. RNA-seq Quantification: quant.sf & tximport](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/07-quantification-tximport.md)
- [8. Differential Expression Analysis — DESeq2](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/08-deseq2.md)
- [9. Visualization: PCA, Heatmap, Volcano, MA Plot](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/09-visualization.md)
- [10. Functional & Pathway Enrichment](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/10-enrichment-analysis.md)
- [11. Writing Up the Project](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/11-reporting.md)
- [Appendix A — Master Command Cheat Sheet](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/appendix-a-cheatsheet.md)
- [Appendix B — Revision Questions](https://github.com/nazninshuktara/Linux-for-Bioinformatics/blob/main/docs/appendix-b-revision.md)

## 🚀 Quick Start
To clone this repository locally:
git clone [https://github.com/nazninshuktara/Linux-for-Bioinformatics.git](https://github.com/nazninshuktara/Linux-for-Bioinformatics.git)
cd Linux-for-Bioinformatics

## 📜 License
This project is licensed under the MIT License - see the LICENSE file for details.
