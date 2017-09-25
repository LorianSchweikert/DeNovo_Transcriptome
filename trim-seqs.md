### This code is for trimming the raw PE reads from Illumina sequencing of L. maximus skin and retina. Reads from skin and retinal tissues were trimmed using two seperate scripts.

```bash
#Move into the appropriate folder
cd /dscrhome/les84/Trimming

#Begin trimming reads from L. maximus skin
  ../PROGRAMS/trimmomatic \
  PE \
  -threads 4 \
  -phred33 \
  ~/FastqFiles_NS087_Schweikert/Schweikert_skin_S11_R1_001.fastq.gz \
  ~/FastqFiles_NS087_Schweikert/Schweikert_skin_S11_R2_001.fastq.gz \
  Skin_FTrimmed_Paired.fastq.gz \
  Skin_FTrimmed_Unpaired.fastq.gz \
  Skin_RTrimmed_Paired.fastq.gz \
  Skin_RTrimmed_Unpaired.fastq.gz \
  ILLUMINACLIP:~/PROGRAMS/Trimmomatic-0.35/adapters/all.fa:2:30:7 \
  LEADING:20 \
  TRAILING:20 \
  SLIDINGWINDOW:4:20 \
  AVGQUAL:20 \
  MINLEN:50
```
```bash
#Then trim reads from L. maximus skin
   ../PROGRAMS/trimmomatic \
   PE \
   -threads 4 \
   -phred33 \
   ~/FastqFiles_NS087_Schweikert/Schweikert_retina_S12_R1_001.fastq.gz \
   ~/FastqFiles_NS087_Schweikert/Schweikert_retina_S12_R2_001.fastq.gz \
   Retina_FTrimmed_Paired.fastq.gz \
   Retina_FTrimmed_Unpaired.fastq.gz \
   Retina_RTrimmed_Paired.fastq.gz \
   Retina_RTrimmed_Unpaired.fastq.gz \
   ILLUMINACLIP:~/PROGRAMS/Trimmomatic-0.35/adapters/all.fa:2:30:7 \
   LEADING:20 \
   TRAILING:20 \
   SLIDINGWINDOW:4:20 \
   AVGQUAL:20 \
   MINLEN:50
```

### Description of the parameters:
- PE :: the input data are paired-end reads
- -threads 4 :: four CPUs (threads) are used
- -phred33 :: score value of data required for trimming step
- input name of the forward reads fileinput name of the reverse reads file
- output name of the trimmed and paired forward file
- output name of the trimmed and unpaired forward file
- output name of the trimmed and reverse forward file
- output name of the trimmed and reverse forward file
- ILLUMINACLIP:file:2:30:7 :: fasta file of adapter sequences to trim
- 2 :: seedMismatches: specifies the maximum mismatch count which will still allow a full match to be performed
- 30 :: palindromeClipThreshold: specifies how accurate the match between the two 'adapter ligated' reads must be for PE palindrome read alignment
- 7 :: simpleClipThreshold: specifies how accurate the match between any adapter etc. sequence must be against a read
- LEADING:20 :: trim bases from start of the read with a quality < 20
- TRAILING:20 :: trim bases from end of the read with a quality < 20
- SLIDINGWINDOW:4:20 :: using a 4-base window, remove the last base if average quality < 2
- AVGQUAL:20 :: remove the entire read if the average quality is < 20
- MINLEN:50 :: remove reads less than 50 bases after quality trimming
