# 5. Project Organization & Sample Naming
---

## 5.1 Recommended Directory Structure

```bash
bulk_RNAseq/
├── data/
│   ├── raw/            # original FASTQ — never modify
│   ├── trimmed/        # trimmed reads
│   ├── quant/           # Salmon output, one folder per sample
│   │   ├── Sample01/quant.sf
│   │   └── Sample02/quant.sf
│   └── metadata/
│       ├── metadata.csv
│       └── tx2gene.csv
├── scripts/
├── results/
│   ├── fastqc/  multiqc/  salmon/  deseq2/  enrichment/
├── figures/
└── docs/
```

**Golden rule: never modify data/raw/ — keep the original files untouched.**

## 5.2 Sample Naming Conventions

- Single-end: Sample01.fastq.gz

- Paired-end: Sample01\_R1.fastq.gz and Sample01\_R2.fastq.gz — R1/R2 must always refer to the same biological sample.

- Metadata sample IDs must exactly match the FASTQ / `quant.sf` sample identifiers (e.g. S01, not Sample\_1 or Patient01) unless you deliberately maintain an explicit mapping table.

> ⚠️ **Warning:** Never accidentally mix R1 from one sample with R2 from another, and never let a metadata sample ID silently mismatch a data file's sample ID — both are common, serious sources of analysis error.
