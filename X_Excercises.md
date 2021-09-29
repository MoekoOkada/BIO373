# Exercises

## 1. Command line

GFF file (Ahal.gff) located in `/scratch/bio373_2020/data/SNPcalling/reference`

GFF files contains information on features of a sequence: genes, introns, etc. Take a look and familiarize yourself with the format.

<http://www.ensembl.org/info/website/upload/gff.html>

1. Create symbolic link of this GFF file to your directory.
2. How many lines are in Ahal.gff? Characters?
3. How many of each type of feature (column 3) are there?
4. How many CDS’s are there within scaffold_1? Beware! There’s also scaffold_10, scaffold_11, etc

### Extra practice 1

Zip the file and modify your commands to answer the same questions!

```bash
$ gzip Ahal.gff
```

Answers can be found on the server here: `/scratch/bio373_2021/data/SNPcalling/answers/CommandLineAnswers.md`

* * *

## 2. Set up environment

1. How many sequences are there in the reference sequence file (MedtrChr2.fa)?

2. How many reads does each fastq file have (\*\_R1.fastq.gz)? Does each sample have the same number of R1 and R2 reads? (Caution: Q scores can be + or @)

3. How many bases are in the reference sequence? How many missing bases (N)? Don’t forget ‘\\n’ is considered a character!

## Answers

On the server: `/scratch/bio373_2020/data/SNPcalling/answers/EnvironmentAnswers.md`

* * *

## 3. Mapping

These exercises are really just designed to try to get you to understand what mapping does with the reads, the almost dizzying amount of information encoded in the files, and what a potential variant might look like in a BAM file. These are fairly detailed.

Bitwise flag meaning: <https://broadinstitute.github.io/picard/explain-flags.html>

1. During mapping, each read is tagged with a bitwise flag that contains information about the read. Find bitwise flags for a few reads in any bam file and decode them using the link above. On the website, you can check only one box at a time to see what an individual property’s value is.

2. Using samtools tview, go to chr2:7271 in 516950.sorted.bam by pressing ‘g’ then typing chr2:7271[Enter]. Compare that with chr2:1018541. Why do you think they are different in terms of coverage and mapping quality? Find a few other areas that look interesting to you and take note of their position if you'd like to go back after the SNP calling step and see if a SNP was indeed called.

3. Do these regions look the same in sample 660389?

### Extra practice 2

Practice writing bash script to run the alignment steps on both genotypes. To encourage organization and reproducibility, make a directory to keep your script(s) in and run from there :)

### Answer

In `/scratch/bio373_2020/data/SNPcalling/answers/MappingAnswers.md`

## 4. Variant call part 1

1. How many unique bitwise flags are there in 516950.sorted.bam file? The dedupped bam file (516950.dedup.bam)? How many reads were marked as duplicates? (Hint: the flags are the sum of the value of each individual property assigned to a read; duplicate = 1024)

2. Here, we'll look in the VCF (04_raw_variants.vcf.gz) and take note of the information contained in the file (which is an overwhelming amount!). I like to get to the variants by searching for CHROM (`/CHROM`). You can look at any SNP, but I suggest searching for 7317, then 1018580. Those sites correspond to where we looked at in the BAM file in the mapping exercises. Take note of the variant quality (QD in INFO field). For an individual, take note of the genotype quality (GQ) and depth (AD and DP) as well. If you'd like, view the BAM file again using `samtools tview` and observe how the results we get from GATK compare to what you can see at those positions in a BAM file. Are the genotypes what you would expect just by looking at the BAM file?  

3. How many variants were discovered in this sample set?

4. Count the number of each genotype (0/0, 0/1, etc) for each sample.

## 5. Variant call part 2

1. Here, we'll look in the filtered VCF (05_variants_filtered.vcf.gz). This time, see if you notice what changed after the filtration step. For example, the FILTER field should now have a value (not just '.'). If you search for `FT`, you'll see in either samples the GT should have become a NoCall (./.).

2. Count the number of variants that failed each of the filters we applied (those applied to stats in the INFO field).

3. Count the number of genotypes that passed the genotype filter (GQ) for each sample. This one's a bit tricky due to the `FT` behavior noted above!
