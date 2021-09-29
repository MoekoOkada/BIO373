# Answers of Excercises: Mapping

## Question 1

> During mapping, each read is tagged with a bitwise flag that contains information about the read. Find bitwise flags for a few reads in any bam file and decode them using the link above. On the website, you can check only one box at a time to see what an individual property’s value is.

While you can use `grep` to this task, it would have to read the whole file before returning the results. A more efficient way in this case is to pipe the output from `samtools view` to `less`. The bit flag is in the second column.

```bash
$ samtools view 01_aligned/516950.sorted.bam | less
```

* * *

## Question 2

> Using samtools tview, go to chr2:7271 in 516950.sorted.bam by pressing ‘g’ then typing chr2:7271[Enter]. Compare that with chr2:1018541. Why do you think they are different in terms of coverage and mapping quality? Find a few other areas that look interesting to you and take note of their position if you'd like to go back after the SNP calling step and see if a SNP was indeed called.

To open a file, you need specify both the bam file and the reference.

```bash
$ samtools tview 01_aligned/516950.sorted.bam 00_input/MedtrChr2.fa
```

The mapping quality in chr2:1018541 is lower than chr2:7271. There are some possibility.

- Genes can be duplicated in the genome. The second gene is a gene that has multiple copies in the genome, and it is possible that reads that should have been mapped elsewhere were mapped to this region.
- The gene locates on chr2:1018541 is putative cyanidin 3-O-galactoside 2''-O-xylosyltransferase FGGT1. This is related to anthocyanin production. There might be a lot of copies of this genes with variants in the _Medicago trancatula_ genome.
- The first region (chr2:7271) codes putative double-strand break repair protein MRE11. This might be single copy gene in the _Medicago trancatula_ genome. This is important gene. If gene duplication does occur, mutations are unlikely to accumulate.

* * *

## Question 3

> Do these regions look the same in sample 660389?

Yes, both regions look quite similar in 660389.

* * *

## Extra practice 2

> Practice writing bash script to run the alignment steps on both genotypes. To encourage organization and reproducibility, make a directory to keep your script(s) in and run from there :)

Example of bash script

```bash
#!/bin/bash
bio373=/sratch/bio373_2021
workdir=${bio373}/moeko
acc=(516950 660389)

cd ${workdir}
pwd

# modules
source /usr/local/ngseq/etc/lmod_profile
module load Aligner/BWA/0.7.17
module load Tools/samtools/1.11

# index
bwa index 00_input/MedtrChr2.fa

# mapping
for acc in "516950" "660389"; do
    bwa mem -M -t 2 -R "@RG\tID:CAV90ANXX.6\tPL:Illumina\tLB:${acc}\tSM:${acc}" \
        00_input/MedtrChr2.fa 00_input/${acc}_chr2_R{1,2}.fastq.gz | samtools sort -m 16G \
        -T /scratch/bio373_2021/moeko -o 01_aligned/${acc}.sorted.bam
done
```
