# 2. Linux Fundamentals
---

## 2.1 Navigation

| Command | Meaning | Example |
| --- | --- | --- |
| `pwd` | Print Working Directory — shows where you currently are | `pwd`  →  /home/username |
| `ls` | List files/folders in the current location | ls |
| `cd <dir>` | Change directory | cd Documents |
| `cd ..` | Go one level up | cd .. |
| `cd ~` | Go to home directory | cd \~ |

## 2.2 File & Directory Management

| Command | Meaning |
| --- | --- |
| `mkdir name` | Create a directory |
| `touch file` | Create an empty file |
| `cp src dst` | Copy a file |
| `mv old new` | Move / rename a file |
| `rm file` | Remove a file (does not go to a recycle bin — be careful) |
| `clear` | Clear the terminal screen |
| `history` | Show previously used commands |

> ⚠️ **Warning:** rm permanently deletes files. Double-check the filename before running it.

## 2.3 Viewing File Contents

| Command | Purpose | Example |
| --- | --- | --- |
| `cat file` | Display full file contents | cat sequence.fasta |
| `head file` | Show the first 10 lines | head -n 5 sequence.fasta |
| `tail file` | Show the last 10 lines | tail -n 5 sequence.fasta |
| `wc file` | Count lines, words, characters | wc -l sequence.fasta |
| `less file` | Browse a large file page by page (q to quit) | less genes.fasta |

> **Note:** wc -l file.fasta returning 20 means the file has 20 lines — not 20 sequences.

## 2.4 Searching Text — grep

`grep` searches for a pattern inside a file.

```bash
grep "COVID" metadata.txt          # basic search
grep ">" sequence.fasta            # find FASTA headers
grep -i "gene" genes.fasta         # -i = ignore case
grep -r "COVID".                   # -r = recursive, search sub-directories too
grep -rin "COVID".                 # -r recursive, -i ignore case, -n show line numbers
```

> **Note:** `grep` ">" genes.fasta | wc -l counts FASTA sequences by counting header lines.

## 2.5 Extracting Columns — cut

cut extracts specific columns/fields from a tab- (or otherwise) delimited file.

```bash
cut -f 1 metadata.txt      # extract column 1
cut -f 2 metadata.txt      # extract column 2 (e.g. Condition)
```

## 2.6 Sorting & Uniqueness — sort, uniq

```bash
cut -f 2 metadata.txt | sort            # alphabetically sort the Condition column
cut -f 2 metadata.txt | sort | uniq     # keep only unique values
cut -f 2 metadata.txt | sort | uniq -c  # count how many of each unique value
```

`uniq` only removes consecutive duplicates, which is why the data is sorted first.

## 2.7 The Pipe |

The pipe passes the output of one command as the input of the next: command1 | command2. Chains of pipes (command1 | command2 | command3) are extremely common in bioinformatics, e.g.:

```bash
cut -f 2 metadata.txt | tail -n +2 | sort | uniq -c
```

Reading left to right: extract the Condition column → drop the header → sort → count occurrences of each group.

## 2.8 awk — Column-Based Processing

`awk` is one of the most useful tools for tabular biological data (metadata, expression tables, annotation/BED/GFF files).

```bash
awk '{print $1}' file.txt                           # $1, $2, $3 = 1st/2nd/3rd column; $0 = whole line
awk '$2=="COVID" {print}' metadata.txt              # print rows where column 2 is COVID
awk '$2=="COVID" {print $1}' metadata.txt           # print only the sample IDs of COVID rows
awk 'NR>1 {print}' metadata.txt                     # NR = current line number; NR>1 skips the header
awk 'NR>1 && $2=="COVID" {count++} END {print count}' metadata.txt        # count COVID samples
```

## 2.9 sed — Stream Editing

```bash
sed -n '2,4p' metadata.txt                # print lines 2-4 only
sed '1d' metadata.txt                     # delete (skip) line 1 — the header
sed 's/COVID/SARS-CoV-2/g' metadata.txt   # s = substitute, g = globally (every match on a line)
```

> **Note:** `sed` does not modify the original file unless you add the -i flag.

## 2.10 Combining Commands — a Worked Example

Given a `metadata.txt` with columns Sample\_ID, Condition, Sex, count samples per condition:

```bash
cut -f 2 metadata.txt | tail -n +2 | sort | uniq -c
```

Step by step: cut extracts the Condition column → tail -n +2 removes the header → sort groups identical values together → `uniq` -c counts each group. This pattern (extract → sort → deduplicate/count) is one of the most common idioms in bioinformatics scripting.

## 2.11 Finding Files — find

```bash
find .                             # everything under the current directory
find . -type f                     # files only
find . -type d                     # directories only
find . -name "*.fasta"             # by name pattern
find . -name "*.fastq.gz"          # compressed FASTQ files
find . -name "quant.sf"            # locate every Salmon quant.sf in a project
find data/quant -name "quant.sf" | wc -l   # count how many samples were quantified
```

## 2.12 Recursive Search Inside Files

```bash
grep -r "COVID" .        # search every file under the current directory
grep -ri "covid" .       # ignore case as well
grep -rin "COVID" .      # also print line numbers
```

This is how you search for a gene or keyword across an entire project without opening every file manually, e.g. `grep` -r "TP53" data/.

## 2.13 File Sizes and Listing Details — ls -l / -lh / -la / -lhS

| Flag | Meaning |
| --- | --- |
| `ls -lh` | Long listing with human-readable sizes (2.5M instead of 2621440) |
| `ls -la` | Include hidden files |
| `ls -lhS` | Sort by file size — useful for spotting very large sequencing files |

## 2.14 File Permissions & chmod

A permissions string such as -rwxr-xr-x is read as three groups: Owner / Group / Others, each made of r (read), w (write), x (execute).

```bash
ls -l                     # view permissions
chmod +x script.sh        # make a script executable
./script.sh               # run it
```

Numeric shortcuts: r=4, w=2, x=1, so rwx=7, rw-=6, r-x=5, r--=4.

| chmod value | Meaning |
| --- | --- |
| `755` | Owner: rwx (7) · Group: r-x (5) · Others: r-x (5) — typical for scripts |
| `644` | Owner: rw- (6) · Group: r-- (4) · Others: r-- (4) — typical for data files |

> ⚠️ **Warning:** Avoid `chmod` 777 — it grants full access to everyone and is rarely necessary.

## 2.15 Output Redirection — >, >>, and tee

| Operator | Meaning |
| --- | --- |
| `>` | Send command output to a file, overwriting it |
| `>>` | Append command output to a file |
| `cmd | tee file` | Show output on screen AND save it to a file |
| `cmd | tee -a file` | Same as above, but append instead of overwrite |

```bash
awk 'NR>1 {print $2}' metadata.txt | sort | uniq -c > sample_summary.txt
grep ">" genes.fasta > gene_names.txt
```
