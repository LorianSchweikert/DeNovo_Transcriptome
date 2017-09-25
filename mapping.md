This code is for mapping the trimmed sequence reads of L. maximus skin and retina back to the assembled transcriptome. Reads from skin and retinal tissues were mapped in two seperate scripts.

## Move into the appropriate folder
cd /dscrhome/les84/Mapping/rsem

## Make folder for skin results
mkdir Skin_rsem 

 Trinityv2.3.2/util/align_and_estimate_abundance.pl \
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

/dscrhome/frr6/PROGRAMS/Trinityv2.3.2/util/align_and_estimate_abundance.pl \
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

Description of the parameters:
--transcripts :: name of input assembly fasta file
--seqType fq :: sequences in fastq format
--left :: input name of forward trimmed paired file
--right :: input name of reverse trimmed paird file
--est_method :: abundance estimation method; alignment based RSEM
--output_dir :: where to place output files--aln_method :: alignment method; required if aligmnent based estimation method
--SS_lib_type RF :: strand specific library type
--thread_count 8 :: 8 CPUs (threads) will be used
--trinity_mode :: setting to automatically generate the gene_trans_map and use it
--prep_reference :: builds target index--output_prefix :: prefix for output files

This next chuck of code is for filtering by expression level. A contig minimum expression level of 1 transcript per million was chosen for the current study.

##Run a filtering program built into trinity
/dscrhome/frr6/PROGRAMS/Trinityv2.3.2/util/filter_low_expr_transcripts.pl 
--matrix isoforms.counts.matrix
--t ~/Assembly/trinity_out_dir/Trinity-VecCon.fasta
--min_expr_any 1 --trinity_mode > Trinity-VecCon-TPM1.fasta

Alternatively a list of contigs to be kept can be filtered in R. Seqtk can be used to put assembly in 2-line format and desired contigs can be filtered out.
