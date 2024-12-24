# Step 9: Assemble and Quantify Expressed Genes and Transcripts with StringTie

After converting your RNA-Seq alignment files into sorted BAM format, the next step is to assemble and quantify the genes and transcripts to understand which genes are active and how much they are expressed in each sample. This is done using a tool called StringTie.

What is StringTie? StringTie is a software tool designed to Assemble transcripts (the parts of genes that are being actively used) from RNA-Seq data and quantify their expression levels (how much RNA is produced for each transcript). This step is important because it turns raw RNA-Seq data into biologically meaningful results.

How to Use StringTie? For each of your 12 RNA-Seq samples, you'll provide: a BAM file: This contains the alignment data of RNA-Seq reads to the genome, a reference annotation file (GTF): This tells StringTie about known genes and transcripts in the genome. It uses this to guide the assembly.

StringTie will assemble the RNA sequences that match genes and transcripts, calculate the expression level for each transcript.

## Running StringTie for a Single Sample
### Explanation:
   -p 12: Uses 12 CPU threads.
   
   -G chrX.gtf: Reference annotation file to guide assembly.
   
   -o ERR188044_chrX.gtf: Output file for the assembled transcripts and quantified data.
   
   -l ERR188044: Label for the sample.
   
  ERR188044_chrX.bam: Input BAM file containing RNA-Seq alignment data.
   
  
```bash
stringtie -p 12 -G chrX_data/genes/chrX.gtf -o ERR188044_chrX.gtf -l ERR188044 ERR188044_chrX.bam
```
```bash
stringtie -p 12 -G chrX_data/genes/chrX.gtf -o ERR188104_chrX.gtf -l ERR188104 ERR188104_chrX.bam
```
```bash
stringtie -p 12 -G chrX_data/genes/chrX.gtf -o ERR188234_chrX.gtf -l ERR188234 ERR188234_chrX.bam
```
```bash
stringtie -p 12 -G chrX_data/genes/chrX.gtf -o ERR188245_chrX.gtf -l ERR188245 ERR188245_chrX.bam
```
```bash
stringtie -p 12 -G chrX_data/genes/chrX.gtf -o ERR188257_chrX.gtf -l ERR188257 ERR188257_chrX.bam
```
```bash
stringtie -p 12 -G chrX_data/genes/chrX.gtf -o ERR188273_chrX.gtf -l ERR188273 ERR188273_chrX.bam
```
```bash
stringtie -p 12 -G chrX_data/genes/chrX.gtf -o ERR188337_chrX.gtf -l ERR188337 ERR188337_chrX.bam
```
```bash
stringtie -p 12 -G chrX_data/genes/chrX.gtf -o ERR188383_chrX.gtf -l ERR188383 ERR188383_chrX.bam
```
```bash
stringtie -p 12 -G chrX_data/genes/chrX.gtf -o ERR188401_chrX.gtf -l ERR188401 ERR188401_chrX.bam
```
```bash
stringtie -p 12 -G chrX_data/genes/chrX.gtf -o ERR188428_chrX.gtf -l ERR188428 ERR188428_chrX.bam
```
```bash
stringtie -p 12 -G chrX_data/genes/chrX.gtf -o ERR188454_chrX.gtf -l ERR188454 ERR188454_chrX.bam
```
```bash
stringtie -p 12 -G chrX_data/genes/chrX.gtf -o ERR204916_chrX.gtf -l ERR204916 ERR204916_chrX.bam
```
---

## Step 10: Merge transcripts from all samples
## The next step is to merge the transcripts from all samples using StringTie. This combines the individual transcript assemblies into a unified set of transcripts that represents all the samples
The file named mergelist.txt containing the names of all the .gtf files. Then, run StringTie Merge: use stringtie --merge to combine the transcripts. 

--merge: Instructs StringTie to merge transcripts.

-p 12: Specifies 12 threads for faster processing.

-G chrX.gtf: Provides the reference annotation file.

-o merged.gtf: Specifies the output file name for the merged transcripts.

mergelist.txt: Input file containing the list of all .gtf files.

## Output: The output will be a single merged.gtf file that contains the unified set of transcripts across all samples.

```bash
stringtie --merge -p 12 -G chrX_data/genes/chrX.gtf -o stringtie_merged.gtf chrX_data/mergelist.txt
```
---
## Step 11: Estimate Transcript Abundances Using Ballgown

In this step, you'll use Ballgown, an R package, to estimate the abundance of transcripts based on the GTF files generated by StringTie.

### 11a: Install Ballgown

This will install Ballgown and the necessary dependencies for transcript abundance estimation.

```bash
conda install bioconda::bioconductor-ballgown
```
### 11a: Estimate Transcript Abundances Using Ballgown

You need the following input files: Merged GTF file (merged.gtf) generated from StringTie, StringTie output files (the GTF files for each sample).

-e: Enables transcript expression estimation.

-B: This option tells StringTie to generate Ballgown-compatible output.

-p 12: Uses 12 threads for parallel processing (adjust based on your system’s CPU).

-G stringtie_merged.gtf: Provides the merged reference GTF file from StringTie (generated in Step 10).

-o ballgown/ERR188044/ERR188044_chrX.gtf: The output GTF file for this specific sample.

ERR188044_chrX.bam: The BAM file for the sample.


```bash
stringtie -e -B -p 12 -G stringtie_merged.gtf -o ballgown/ERR188044/ERR188044_chrX.gtf ERR188044_chrX.bam
```
For example, The files listed in the ballgown/ERR188044 directory are part of the output generated by StringTie and are intended for use with the Ballgown package for differential expression analysis. Here’s a brief explanation of each file:

  e2t.ctab: This file contains the mapping between exons and transcripts, used by Ballgown to identify exon-to-transcript relationships.

  e_data.ctab: This file contains the expression data for each exon across samples, including transcript abundance.

  ERR188044_chrX.gtf: This is the GTF file generated by StringTie. It contains the transcript assembly results for your specific sample (ERR188044) on chromosome X.

  i2t.ctab: This file contains the mapping between isoforms and transcripts, used by Ballgown to identify isoform-to-transcript relationships.

  i_data.ctab: This file contains the expression data for each isoform across samples.

  t_data.ctab: This file contains the expression data for each transcript across samples, which is used in Ballgown for downstream analysis.

## Repeat the analysis on rest of files

```bash
stringtie -e -B -p 12 -G stringtie_merged.gtf -o ballgown/ERR188104/ERR188104_chrX.gtf ERR188104_chrX.bam
```
```bash
stringtie -e -B -p 12 -G stringtie_merged.gtf -o ballgown/ERR188245/ERR188245_chrX.gtf ERR188245_chrX.bam
```
```bash
stringtie -e -B -p 12 -G stringtie_merged.gtf -o ballgown/ERR188257/ERR188257_chrX.gtf ERR188257_chrX.bam
```
```bash
stringtie -e -B -p 12 -G stringtie_merged.gtf -o ballgown/ERR188273/ERR188273_chrX.gtf ERR188273_chrX.bam
```
```bash
stringtie -e -B -p 12 -G stringtie_merged.gtf -o ballgown/ERR188337/ERR188337_chrX.gtf ERR188337_chrX.bam
```
```bash
stringtie -e -B -p 12 -G stringtie_merged.gtf -o ballgown/ERR188383/ERR188383_chrX.gtf ERR188383_chrX.bam
```
```bash
stringtie -e -B -p 12 -G stringtie_merged.gtf -o ballgown/ERR188401/ERR188401_chrX.gtf ERR188401_chrX.bam
```
```bash
stringtie -e -B -p 12 -G stringtie_merged.gtf -o ballgown/ERR188428/ERR188428_chrX.gtf ERR188428_chrX.bam
```
```bash
stringtie -e -B -p 12 -G stringtie_merged.gtf -o ballgown/ERR188454/ERR188454_chrX.gtf ERR188454_chrX.bam
```
```bash
stringtie -e -B -p 12 -G stringtie_merged.gtf -o ballgown/ERR204916/ERR204916_chrX.gtf ERR204916_chrX.bam
```
---
### We've successfully completed the RNA-Seq pipeline up to transcript abundance estimation. Typically, companies or sequencing platforms provide the transcript abundance files, which are crucial for downstream analysis. The next step is to focus on ### differential expression, functional annotation, and data visualization. For these tasks, you'll need to excel in R and use tools like Ballgown, DESeq2, and ggplot2 to analyze and interpret the data. With the abundance data, you will be able to find not only genes but also specific transcript isoforms. Continue exploring these tools and apply them to your own datasets. The updated pipeline is now available on GitHub for further exploration.


















