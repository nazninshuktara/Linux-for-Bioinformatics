#🐧 Linux-for-Bioinformatics
Self-study notes on Linux, Bash scripting, and R/Bioconductor tools for bioinformatics.

Linux · Bash scripting · Conda ·
Salmon/tximport · DESeq2 · GO/KEGG/Reactome/GSEA enrichment.

Prepared by Naznin.

## Contents

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

