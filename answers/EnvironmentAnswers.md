# Answers of Excercises: Set up environment

## Question 1

> How many sequences are there in the reference sequence file (MedtrChr2.fa)?

```bash
$ cd /scratch/bio373_2021/moeko/VarVall/
$ grep -c '^>' 00_input/MedtrChr2.fa
1
$
```

`^` means the beginning of the line.

* * *

## Question 2.

Make sure you are in `VarCall` directory.

> 2-1. How many reads does each fastq file have (\*\_R1.fastq.gz)?

Each sequence in fastq file has 4 lines of information.
So, you can count the number of sequences by counting the lines and dividing them by 4.
Before that, you need to enter the input directory. And you have to use some bash script to divide them.

```bash
$ cd 00_input
$ echo $(( $(zcat 516950_chr2_R1.fastq.gz | wc -l) /4 ))
165033
$ echo $(( $(zcat 516950_chr2_R2.fastq.gz | wc -l) /4 ))
165033
$ echo $(( $(zcat 660389_chr2_R1.fastq.gz | wc -l) /4 ))
157539
$ echo $(( $(zcat 660389_chr2_R2.fastq.gz | wc -l) /4 ))
157539
$
```


* * *

> 2. Does each sample have the same number of R1 and R2 reads? (Caution: Q scores can be + or @)

No. The number of leads in R1 does not necessarily match the number of leads in R2. This is due to errors in library creation or in rare cases, the sequencer fails to sequence one side.

* * *

## Question 3

Make sure you are in `VarCall` directory.

> 3-1. How many bases are in the reference sequence?

```bash
$ grep -v "^>" 00_input/MedtrChr2.fa | wc -c
2033335
$
```

New line (`\\n`) is considered a character. So the number of bases is 203334.

* * *

> 3-2. How many missing bases (N)?

Same as above but you need to remove all characters that are not `N`. This can be done by removing the complement to 'N' (`-c`). You have to be careful as some fasta files contain lower-case letters and all shell tools are case-sensitive by default.

```shell
grep -v '^>' 00_input/MedtrChr2.fa | tr -cd 'N' | wc -c
46942
$
```
