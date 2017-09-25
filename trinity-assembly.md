This code is for assembling the trimmed sequence reads from both tissues into a de novo assembly. 

# Move into the appropriate folder
cd /dscrhome/les84/Assembly

# Explain path to trinity required library
export LD_LIBRARY_PATH=/usr/lib64/:$LD_LIBRARY_PATH

# Concatenate compressed forward trimmed reads from both tissues
zcat ../Trimming/Retina_FTrimmed_Paired.fastq.gz ../Trimming/Skin_FTrimmed_Paired.fastq.gz | gzip > forward.fastq.gzecho "Finished concatenating Forward files"

# Concatenate compressed reverse trimmed reads from both tissues
zcat ../Trimming/Retina_RTrimmed_Paired.fastq.gz ../Trimming/Skin_RTrimmed_Paired.fastq.gz | gzip > reverse.fastq.gzecho "Finished concatenating Reverse files"

# Begin de novo assembly
   Trinity \
   --seqType fq \
   --max_memory 192G \
   
   --left forward.fastq.gz \
   --right reverse.fastq.gz \
   --CPU 16

Description of the parameters:
   --seqType fq :: sequences are provided in fastq format
   --max_memory 192G :: the largest amount of memory of which trinity may take advantage
   --SS_lib_type RF :: strand-specific RNA-Seq read orientation; first read (/1) of fragment pair is sequenced as anti-sense (reverse(R)), and second read (/2) is in the sense strand (forward(F)); known from sequencing method
   --min_contig_length 200 :: minimum assembled contig length to report
   --verbose :: provide additional job status info during the run
   input name of the forward concatenated file
   input name of the reverse concatenated file
   --CPU 16 :: 16 CPUs (threads) are used
   
  This next chuck of code is for screening the transcriptome for vector contamination. This is required before submission to NCBI. Remove any vector contaminants from transcriptome, then save as Trinity-VecCon.fasta
  
# Make Folder for Univec Blasting
mkdir /dscr/les84/UNIVEC
cd UNIVEC

# Download Univect Database 
wget ftp://ftp.ncbi.nlm.nih.gov/pub/UniVec/UniVec_Core

# Make Univec Database   
   makeblastdb \
   -in UniVec_Core \
   -title UNIVEC \
   -out UNIVEC \
   -dbtype nucl

# Blast contigs to Univec
    database blastn \
   -query ~/Assembly/trinity_out_dir/Trinity.fas \
   -db UNIVEC \
   -num_threads 4 \
   -outfmt 6 \
   -out vector-screen.tsv
