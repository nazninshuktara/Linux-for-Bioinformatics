# 🐧 Linux-for-Bioinformatics
Self-study notes on Linux, Bash scripting, and R/Bioconductor tools for bioinformatics.
> Prepared by **Naznin**.
---

## 📌 Table of Contents
- [Overview](#overview)
- [Topics Covered](#topics-covered)
  - [1. Linux Fundamentals](#1-linux-fundamentals)
  - [2. Biological File Formats & QC](#2-biological-file-formats--qc)
  - [3. Bash Scripting for Pipelines](#3-bash-scripting-for-pipelines)
  - [4. Environment Management (Conda/Mamba)](#4-environment-management-condamamba)
  - [5. RNA-seq Quantification (Salmon & tximport)](#5-rna-seq-quantification-salmon--tximport)
  - [6. Differential Expression Analysis (DESeq2)](#6-differential-expression-analysis-deseq2)
  - [7. Visualization & Pathway Enrichment](#7-visualization--pathway-enrichment)
- [Repository Structure](#repository-structure)
- [Quick Start](#quick-start)
- [License](#license)

---

## 📖 Overview
This repository serves as a personal knowledge base and practical guide for using Linux CLI, Bash automation, and R/Bioconductor environments for bioinformatics data analysis (e.g., RNA-seq, sequence analysis).

---



## 🛠️ Topics Covered

### 🐧 1. Linux Basics
- Directory navigation (`cd`, `ls`, `pwd`)
- File manipulation (`mkdir`, `cp`, `mv`, `rm`)
- File viewing & processing (`cat`, `head`, `tail`, `grep`, `awk`, `sed`)
- File permissions & environment variables (`chmod`, `PATH`)

### 📜 2. Bash Scripting for Bioinformatics
- Shell script fundamentals (`#!/bin/bash`)
- Loops and conditionals for batch processing
- Writing automated workflows for raw sequencing data (.fastq / .fasta)

### 🔬 3. Bioinformatics CLI Tools
- Quality control: `FastQC`, `MultiQC`
- Trimming & filtering: `Trimmomatic`, `fastp`
- Alignment & SAM/BAM manipulation: `HISAT2`, `STAR`, `samtools`
- Genomic intervals: `bedtools`

### 📊 4. R & Bioconductor
- Installing Bioconductor packages (`BiocManager`)
- Differential expression analysis concepts (e.g., DESeq2)
- Visualization using `ggplot2` and `pheatmap`

---

## 📁 Repository Structure
```text
.
├── 01_Linux_Basics/
├── 02_Bash_Scripting/
├── 03_Bioinformatics_Tools/
├── 04_R_Bioconductor/
├── scripts/
├── .gitignore
├── LICENSE
└── README.md
```


## 📌 Table of Contents

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
- [Appendix B — Revision & Interview Questions](docs/appendix-b-interview-qa.md)

## How to use this repo

Each section in `docs/` can be read on its own, or in order as a course. `Appendix A` is a
one-page command cheat sheet; `Appendix B` collects revision / interview questions for every
topic covered.

## Scope

These notes accompany a bulk RNA-seq workflow: `Salmon → tximport → DESeq2 → PCA/Heatmap/Volcano/MA →
GO/KEGG/Reactome/GSEA`. Code blocks are illustrative — check tool versions and reference
files before running them against real data.

