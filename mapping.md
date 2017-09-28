### This code is for mapping the trimmed sequence reads of L. maximus skin and retina back to the assembled transcriptome. Reads from skin and retinal tissues were mapped in two seperate scripts.

```bash
##To begin mapping, you need to use Bowtie2 to build an index of files (with the same base name) that the mapping script can draw from.

/dcsrhome/les84/bin/Bowtie2-build ~/Assembly/trinity_out_dir/Trinity.fasta Trinity_assembly

## Move into the appropriate folder to begin mapping
cd /dscrhome/les84/Mapping/rsem

## Make folder for skin results
mkdir Skin_rsem 

 ~/PROGRAMS/Trinityv2.3.2/util/align_and_estimate_abundance.pl \
 --transcripts ~/Assembly/trinity_out_dir/Trinity.fasta \
 --seqType fq \--left /dscrhome/les84/Trimming/Skin_FTrimmed_Paired.fastq.gz \
 --right /dscrhome/les84/Trimming/Skin_RTrimmed_Paired.fastq.gz \
 --est_method RSEM \--output_dir ./Skin_rsem \
 --aln_method bowtie2 \--SS_lib_type RF \
 --thread_count 8 \
 --trinity_mode \
 --prep_reference \
 --output_prefix Skin_rsem
 
## Make folder for retina results
mkdir Retina_rsem

~/PROGRAMS/Trinityv2.3.2/util/align_and_estimate_abundance.pl \
--transcripts ~/Assembly/trinity_out_dir/Trinity.fasta \
--seqType fq \
--left /dscrhome/les84/Trimming/Retina_FTrimmed_Paired.fastq.gz \
--right /dscrhome/les84/Trimming/Retina_RTrimmed_Paired.fastq.gz \
--est_method RSEM \
--output_dir ./Retina_rsem \
--aln_method bowtie2 \
--SS_lib_type RF \
--thread_count 8 \
--trinity_mode \
--prep_reference \
--output_prefix Retina_rsem
```

Description of the parameters:
- --transcripts :: name of input assembly fasta file
- --seqType fq :: sequences in fastq format
- --left :: input name of forward trimmed paired file
- --right :: input name of reverse trimmed paird file
- --est_method :: abundance estimation method; alignment based RSEM
- --output_dir :: where to place output files--aln_method :: alignment method; required if aligmnent based estimation method
- --SS_lib_type RF :: strand specific library type
- --thread_count 8 :: 8 CPUs (threads) will be used
- --trinity_mode :: setting to automatically generate the gene_trans_map and use it
- --prep_reference :: builds target index--output_prefix :: prefix for output files

### This next chuck of code is for filtering your assembly by an expression minimum. A gene minimum expression level of 1 transcript per million was chosen for the current study.The mapping step will output isoforms.matrix and genes.matrix expression files. Use R to filter contig names out into a temporary list file, Contigs_keep.list, for isoforms or genes that meet your expression minimum criterion.

```bash
#First, format your assembly file into two lines per sequence using the seqtk tool. The -l0 flag allows an extremely large number of bases to be included on a single line.

seqtk seq -l0 Trinity-VecCon.fasta > Two_Line.fasta

#Next, filter this two line version of the assembly to only include the contigs you wish to keep 

c=1
while read line
do
grep -A1 "$line " Two_Line.fasta
>> Trinity-VecCon-GeneTPM1.fasta
echo "finished $c"
c=$(( $c + 1 ))
done < ~/Contigs_keep.list
```

