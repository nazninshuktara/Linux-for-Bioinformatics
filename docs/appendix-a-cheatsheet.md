# Appendix A — Master Command Cheat Sheet
---

## A.1 Linux / Navigation / Files

| Command | Purpose |
| --- | --- |
| `pwd` | Show current directory |
| `ls / ls -lh / ls -la / ls -lhS` | List files (readable sizes / hidden / by size) |
| `cd dir / cd .. / cd ~` | Change directory / up one level / home |
| `mkdir / mkdir -p` | Create directory (with parents) |
| `touch` | Create empty file |
| `cp / mv / rm` | Copy / move-rename / delete |
| `chmod +x / chmod 755 / 644` | Make executable / set permissions |

## A.2 Viewing & Text Processing

| Command | Purpose |
| --- | --- |
| `cat / head -n / tail -n / less` | View whole file / start / end / paginate |
| `wc -l` | Count lines |
| `grep / grep -i / -r / -n` | Search text (case-insens. / recursive / line no.) |
| `cut -f` | Extract a column |
| `sort / uniq / uniq -c` | Sort / dedupe / count occurrences |
| `awk '{print $N}' / awk 'cond {action}'` | Column extraction / conditional processing |
| `sed -n 'Np' / sed '1d' / sed 's/a/b/g'` | Print line N / delete line 1 / substitute |
| `find . -name / -type f/d` | Find files by name / type |

## A.3 Compression & FASTQ

| Command | Purpose |
| --- | --- |
| `gzip / gunzip` | Compress / decompress |
| `zcat / zless / zgrep` | Read / page / search a .gz file |
| `echo $(( $(zcat f.gz | wc -l) / 4 ))` | Count reads in a FASTQ(.gz) |

## A.4 Bash Scripting

| Syntax | Purpose |
| --- | --- |
| `var="value" / echo $var` | Set / print a variable |
| `for x in list; do …; done` | Loop |
| `if [ cond ]; then …; else …; fi` | Conditional |
| `-f -d -e -r -w -x` | File tests (exists as file / dir / path / readable / writable / executable) |
| `func(){ …; }` | Define a function |
| `$1 $2 $# "$@"` | Arguments / count / all arguments |
| `set -euo pipefail` | Fail fast on errors / undefined vars / pipe failures |
| `${var%pattern}` | Strip a suffix pattern from a variable |
| `cmd > file / >> file / | tee -a file` | Redirect (overwrite / append) / show + append |

## A.5 Conda

| Command | Purpose |
| --- | --- |
| `conda create -n name / activate / deactivate` | Create / enter / leave an environment |
| `conda install -c bioconda tool` | Install a bioinformatics tool |
| `conda list / conda list | grep tool` | List packages / check one |
| `conda env export > file.yml / conda env create -f file.yml` | Save / recreate an environment |

## A.6 R / RNA-seq Workflow

| Function | Purpose |
| --- | --- |
| `tximport(files, type="salmon", tx2gene=...)` | Import Salmon `quant.sf` into a gene-level matrix |
| `DESeqDataSetFromTximport(txi, colData, design)` | Build the `DESeq2` object |
| `DESeq(dds) / results(dds)` | Run the model / extract results |
| `vst(dds) / plotPCA()` | Variance-stabilize / PCA |
| `pheatmap() / plotMA()` | Heatmap / MA plot |
| `bitr() / enrichGO() / enrichKEGG() / enrichPathway()` | ID conversion / GO / KEGG / Reactome ORA |
| `gseGO() / gseKEGG() / gseaplot2()` | GO / KEGG GSEA / enrichment curve plot |
