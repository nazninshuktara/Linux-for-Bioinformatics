# 4. Bash Scripting
---

## 4.1 Variables

```bash
sample="S01"
echo $sample        # → S01
condition="COVID"
echo "$condition"   # → COVID
```

> ⚠️ **Warning:** No spaces around = : sample="S01" is correct; sample = "S01" is a syntax error.

## 4.2 Your First Script

```bash
#!/bin/bash
echo "Hello Bioinformatics!"
echo "Starting COVID RNA-seq analysis..."
```

```bash
chmod +x hello.sh    # make it executable
./hello.sh           # run it
```

> **Note:** #!/bin/bash (the shebang) tells Linux to interpret the script using Bash.

## 4.3 for Loops

```bash
for sample in S01 S02 S03 S04 S05
do
   echo "Processing $sample"
done
```

Structure: for <variable> in <list> — do <commands using `$variable`> — done.

## 4.4 Wildcards (*)

`*` matches any characters, so it is used to select groups of files:

```bash
ls *.fastq              # every file ending in .fastq
ls *_R1.fastq.gz        # every R1 file
for file in samples/*.fastq
do
   echo "Processing $file"
done
```

Common bioinformatics wildcards: `*.fastq`, `*.fastq.gz`, `*.fasta`, `*.fasta.gz`, `*.txt`, `*.csv`, `*.bam`, `*.sam.`

## 4.5 if / else and Comparisons

```bash
if [ condition ]
then
   command
else
   command
fi
```

```bash
age=25
if [ $age -ge 18 ]
then
   echo "Adult"
else
   echo "Minor"
fi
```

| Operator | Meaning |
| --- | --- |
| `-eq` | equal |
| `-ne` | not equal |
| `-gt` | greater than |
| `-lt` | less than |
| `-ge` | greater or equal |
| `-le` | less or equal |

For text (strings) use = instead of the numeric operators:

```bash
condition="COVID"
if [ "$condition" = "COVID" ]
then
   echo "This is a COVID sample"
fi
```

## 4.6 File Tests

| Test | Meaning |
| --- | --- |
| `-f file` | file exists and is a regular file |
| `-d dir`  | directory exists |
| `-e path` | path exists (file or directory) |
| `-r file` | file is readable |
| `-w file` | file is writable |
| `-x file` | file is executable |

```bash
if [ -f "S01.fastq" ]
then
   echo "File exists"
else
   echo "File does not exist"
fi
```

Combine with a loop to validate every FASTQ file before processing:

```bash
for file in samples/*.fastq
do
   if [ -f "$file" ]
   then
      echo "Found: $file"
   else
      echo "Missing: $file"
   fi
done
```

## 4.7 && and ||

| Operator | Meaning |
| --- | --- |
| `cmd1 && cmd2` | Run cmd2 only if cmd1 succeeds |
| `cmd1 || cmd2` | Run cmd2 only if cmd1 fails |

```bash
mkdir test_folder && echo "Folder created"
cd missing_folder || echo "Folder does not exist"
```

## 4.8 Functions

```bash
greeting()
{
   echo "Welcome to Bioinformatics"
}
greeting     # calling the function
```

Functions become powerful once they accept arguments. Inside a function, `$1`, `$2`, `$3` are the first, second, third argument passed to it:

```bash
show_sample()
{
   echo "Sample: $1"
   echo "Condition: $2"
}
show_sample S01 COVID
```

## 4.9 Command-Line Arguments

A script itself can also accept arguments, so one generic script can replace many hard-coded ones:

```bash
#!/bin/bash
sample=$1
echo "Analyzing sample: $sample"
```

```bash
./sample_info.sh S01     # → Analyzing sample: S01
```

| Symbol | Meaning |
| --- | --- |
| `$1, $2, $3 …` | 1st, 2nd, 3rd … argument |
| `$#` | number of arguments supplied |
| `"$@"` | all arguments, expanded as a list — useful in a for loop |

```bash
#!/bin/bash
echo "Samples provided:"
for sample in "$@"
do
   echo "$sample"
done
```

## 4.10 Defensive Scripting — Checking Arguments and Paths

```bash
#!/bin/bash
if [ $# -lt 2 ]
then
   echo "Usage: ./script.sh INPUT_DIR OUTPUT_DIR"
   exit 1
fi
input_dir=$1
output_dir=$2
if [ ! -d "$input_dir" ]
then
   echo "ERROR: Input directory does not exist."
   exit 1
fi
if [ ! -d "$output_dir" ]
then
   mkdir -p "$output_dir"
fi
```

`$#` is the number of arguments; ! negates a test, so [ ! -d "`$dir`" ] means "the directory does NOT exist".

## 4.11 Logging: >, >>, and tee -a

```bash
echo "Pipeline started" > pipeline.log       # overwrite
echo "Running FastQC" >> pipeline.log         # append
echo "Running FastQC" | tee -a pipeline.log   # print to screen AND append to file
```

## 4.12 set -euo pipefail

Placed near the top of a serious Bash script, this makes it fail loudly instead of silently:

| Flag | Effect |
| --- | --- |
| `-e` | stop the script immediately if any command fails |
| `-u` | treat use of an undefined variable as an error |
| `pipefail` | a failure anywhere inside a pipe (cmd1 \| cmd2) fails the whole pipeline |

> **Note:** Interview-ready answer: "set -euo pipefail makes Bash pipelines safer by stopping on command errors, catching undefined variables, and preventing silent failures inside pipes."

## 4.13 Deriving Sample Names & Pairing R1/R2

`${variable%pattern}` strips a matching pattern from the END of a variable — perfect for turning a filename into a sample ID:

```bash
r1="input/COVID01_R1.fastq.gz"
sample=${r1%_R1.fastq.gz}          # → input/COVID01
r2="${sample}_R2.fastq.gz"         # → input/COVID01_R2.fastq.gz
if [ -f "$r2" ]
then
   echo "PAIR OK: $sample"
else
   echo "WARNING: Missing R2 for $sample"
fi
```

A reusable pair-checking function:

```bash
check_pair()
{
   r1=$1
   r2=$2
   if [ -f "$r1" ] && [ -f "$r2" ]
   then
      echo "PAIR OK"
   else
      echo "PAIR ERROR"
   fi
}
```

## 4.14 A Complete, Defensive RNA-seq QC Script

Putting everything from this chapter together — argument checking, directory checking, R1/R2 pairing, basic FASTQ structure validation (line count divisible by 4), FastQC + MultiQC, and logging:

```bash
#!/bin/bash
set -euo pipefail

check_directory() {
   dir=$1
   if [ ! -d "$dir" ]; then echo "ERROR: Directory does not exist: $dir"; exit 1; fi
}

validate_fastq() {
   file=$1
   line_count=$(zcat "$file" | wc -l)
   if [ $((line_count % 4)) -ne 0 ]; then
      echo "ERROR: Invalid FASTQ structure: $file"; return 1
   fi
   echo "FASTQ structure OK: $file"
}

if [ $# -lt 2 ]; then
   echo "Usage: ./run_pipeline.sh INPUT_DIR OUTPUT_DIR"; exit 1
fi
input_dir=$1
output_dir=$2
check_directory "$input_dir"

mkdir -p "$output_dir/fastqc" "$output_dir/multiqc" logs
log_file="logs/pipeline.log"
echo "RNA-seq FASTQ QC Pipeline" | tee "$log_file"

found_r1=false
for r1 in "$input_dir"/*_R1.fastq.gz; do
   [ -f "$r1" ] || continue
   found_r1=true
   sample=${r1%_R1.fastq.gz}
   r2="${sample}_R2.fastq.gz"
   echo "Processing sample: $sample" | tee -a "$log_file"
   if [ ! -f "$r2" ]; then
      echo "WARNING: Missing R2 for $sample" | tee -a "$log_file"; continue
   fi
   validate_fastq "$r1"; validate_fastq "$r2"
   fastqc "$r1" "$r2" -o "$output_dir/fastqc"
done

if [ "$found_r1" = false ]; then
   echo "ERROR: No R1 FASTQ files found." | tee -a "$log_file"; exit 1
fi

multiqc "$output_dir/fastqc" -o "$output_dir/multiqc"
echo "Pipeline completed." | tee -a "$log_file"
```

> **Note:** Run it as: ./run\_pipeline.sh input results — this single generic script replaces dozens of hand-written per-sample commands, and is close to how real bioinformatics pipelines are structured.
