# 3. Biological File Formats
---

## 3.1 FASTA

FASTA stores biological sequences. A header line starts with > followed by the sequence itself:

```bash
>Human_Gene_1
ATGCGTAGCTAGCTAGCTAG
```

**FASTA = sequence only.**

## 3.2 FASTQ

FASTQ stores a sequence together with per-base quality information. Each read is exactly 4 lines:

```bash
@Read_001
ATGCGTAGCTAG
+
IIIIIIIIIIII
```

| Line | Meaning |
| --- | --- |
| `1` | Read identifier (starts with @) |
| `2` | Nucleotide sequence |
| `3` | Separator (+) |
| `4` | Quality string (one character per base) |

**FASTA = sequence.   FASTQ = sequence + quality.**

FASTQ is the standard input format for RNA-seq, WGS, bacterial genomics, and NGS analysis in general.

## 3.3 Compressed Files — gzip family

Sequencing files are often very large, so they are normally stored compressed (.gz).

| Command | Purpose |
| --- | --- |
| `gzip file` | Compress a file (original is removed): sample.fastq → sample.fastq.gz |
| `gunzip file.gz` | Decompress it back to sample.fastq |
| `zcat file.gz` | Print the decompressed content without creating an extracted file |
| `zless file.gz` | Browse a compressed file page by page |
| `zgrep pattern file.gz` | Search inside a compressed file directly |

```bash
zcat sample.fastq.gz | head -n 8          # inspect first two reads (4 lines each)
zcat sample.fastq.gz | wc -l              # total lines
echo $(( $(zcat sample.fastq.gz | wc -l) / 4 ))     # number of reads (4 lines per read)
zcat genes.fasta.gz | grep "^>" | wc -l             # count FASTA sequences in a compressed file
```

> **Note:** .gz only means the file is compressed — the underlying format (FASTA/FASTQ) is unchanged.

> ⚠️ **Warning:** Repeatedly running `zcat` | wc -l on very large files is fine for learning/QC checks, but is computationally expensive in production pipelines — avoid scanning huge files more than necessary.

## 3.4 Paired-End Sequencing (R1 / R2)

In single-end sequencing each fragment produces one read: Sample01.fastq.gz. In paired-end sequencing each fragment is read from both ends, producing two files per sample:

```bash
Sample01_R1.fastq.gz   # Read 1
Sample01_R2.fastq.gz   # Read 2
```

> ⚠️ **Warning:** R1 and R2 are two reads from the SAME biological sample, not two different samples. Never pair R1 from one sample with R2 from another.

File-count arithmetic: 20 fastq.gz files at 2 files/sample (paired-end) = 10 samples — always check this assumption rather than counting files directly as samples.

## 3.5 Phred Quality Scores

The 4th FASTQ line encodes, per base, the probability that the base call is wrong (Phred quality Q):  Q = −10·log10(P\_error).

| Phred score | Error probability | Approx. accuracy |
| --- | --- | --- |
| `Q10` | 10% | 90% |
| `Q20` | 1% | 99% |
| `Q30` | 0.1% | 99.9% |
| `Q40` | 0.01% | 99.99% |

**Rule of thumb worth memorising: Q30 ≈ 99.9% base-call accuracy.**

Quality often decreases toward the end of a read; the exact pattern depends on the sequencing platform, library prep, and read length. Low-quality bases can cause incorrect alignments, quantification errors, false variants, and unreliable downstream results — hence the QC step below.

## 3.6 Quality Control Tools — FastQC & MultiQC

FastQC checks a single FASTQ file for: per-base and per-sequence quality, sequence length, GC content, adapter contamination, overrepresented sequences, and duplication levels.

MultiQC combines many individual FastQC reports (e.g. 50 samples) into one summary report, so you don't have to open each one by hand.

```bash
fastqc sample_R1.fastq.gz sample_R2.fastq.gz -o results/fastqc
multiqc results/fastqc -o results/multiqc
```
