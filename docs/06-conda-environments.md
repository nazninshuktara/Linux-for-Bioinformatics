# 6. Environment Management — Conda / Mamba
---

## 6.1 Why Environments?

Different projects/tools can require different, conflicting software versions. A `conda` environment is an isolated "box" of software, so installing Tool A (needs Python 3.10) and Tool B (needs Python 3.11) doesn't create a global conflict.

```bash
base
├── bulk-rnaseq     → salmon, fastqc, multiqc, R, DESeq2
└── amr-genomics    → amrfinder, abricate, blast, spades, prokka
```

## 6.2 Core Commands

| Command | Purpose |
| --- | --- |
| `conda --version` | Check Conda is installed |
| `conda info` | Conda information |
| `conda env list` | List all environments |
| `conda create -n name python=3.11` | Create a new environment |
| `conda activate name` | Activate an environment |
| `conda deactivate` | Deactivate the current environment |
| `conda remove -n name --all` | Delete an environment (irreversible — be careful) |
| `conda list` | List packages in the active environment |
| `conda list | grep salmon` | Check whether a specific package is installed |
| `which salmon` | Show the path of the executable actually being used |

## 6.3 Channels and Installing Bioinformatics Tools

Conda fetches packages from repositories called channels. Bioconda is the key channel for biological software.

```bash
conda create -n bulk-rnaseq
conda activate  bulk-rnaseq
conda install -c bioconda fastqc
conda install -c bioconda multiqc
conda install -c bioconda salmon
fastqc --version   &&   multiqc --version   &&   salmon --version
```

> **Note:** If an installation fails, don't randomly install things from other websites — copy the exact error message and troubleshoot from that.

## 6.4 Reproducibility — Exporting and Recreating Environments

This is essential for reproducible research: record exactly which software/versions were used, and let anyone (including future-you) recreate the same environment.

```bash
conda env export > bulk-rnaseq-environment.yml     # save the environment
conda env create -f bulk-rnaseq-environment.yml    # recreate it elsewhere
```

Keep bulk-rnaseq-`environment.yml` inside the project (e.g. alongside docs/) so it travels with the repository.

## 6.5 Conda vs Mamba

Mamba performs the same environment/package-management tasks as Conda (mamba create, mamba install, mamba activate) but is typically faster at solving dependencies. Learn Conda first; Mamba's commands mirror it closely.
