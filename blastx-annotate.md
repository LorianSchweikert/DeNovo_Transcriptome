This code is for annotating the de novo assembly using NCBI's blastx feature. The following code was submitted as a SLURM script to the cluster.

# Make each sequence of assembly its own seperate fasta file to be blasted in parallel
fasta-splitter.pl \
--part-size 1 \
--measure count \
--line-length 0 \
Trinity.fasta

Description of Parameters:
--part-size :: divide into parts of size
--measure :: determine what consistutes 'size' ; i.e., sequence count vs length 
--line-length :: output sequence line length; 0 for single line

# Move into the appropriate folder
cd dscrhome/les84/Annotation/XML

# Setup array job; e "end" is job number multiplied by 475; s "start" is end number minus 475.
e=$(( $SLURM_ARRAY_TASK_ID * 475 ))s=$(( $e - 474 ))

# Begin blasting sequences
for ((f=$s;f<=$e;f++))do
echo "Blasting sequence ${f}"blastx \
-query ~/Annotation/Seqs/Trinity.part-${f}.fasta \
-db /work/frr6/BLASTN/nr \
-outfmt 5 \
-out Trinity.part-${f}.xml \
-evalue 0.001 \
-num_threads 2 \
-max_target_seqs 10
echo "Finished sequence $f"done

# Setup array job 
e=$(( $SLURM_ARRAY_TASK_ID * 56 ))s=$(( $e - 55 ))
# Begin blasting sequences
for ((f=$s;f<=$e;f++))do
file=$(sed -n "$f"p ../Missing.list | sed "s/.xml/.fasta/")file1=$(sed -n "$f"p ../Missing.list)
echo "Blasting sequence ${f}"blastx \   -query ~/Annotation/Seqs/$file \   -db /work/frr6/BLASTN/nr \   -outfmt 5 \   -out ${file1} \   -evalue 0.001 \   -num_threads 2 \   -max_target_seqs 10echo "Finished sequence $f"done

Description of  the parameters:
-query :: input file name; portion of assembly
-db :: input blast database
-outfmt 5 :: selected format for the output
-out Trinity.part-${f}.xml :: output file name; portion of the assembly
-evalue 0.001 :: evalue cut off for accepted blastx results
-num_threads 2 :: 2 CPUs (threads) are used
-max_target_seqs 10 :: only the first 10 hit results will be included in the output

Repeating blastx of contigs for which jobs failed. 

# First, identify which jobs need to be run again
for i in {1..237396} \
do file=Trinity.part-${i}.xml\
file=XML/Trinity.part-${i}.xml \
if [ -e "$file" ] \
then echo "$file" | tee -a Exist.list \
else echo "$file" | tee -a Missing.list\
fi\
done
