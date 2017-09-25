This code is for annotating the de novo assembly using IPRscan.The following code was submitted as a SLURM script to the cluster.

# Move into the appropriate folder
cd /dscrhome/les84/IPR_XML

# Setup array job
e=$(( $SLURM_ARRAY_TASK_ID * 500 ))
s=$(( $e - 499 ))

# Begin iprscan of sequences
for ((f=$s;f<=$e;f++))
do
echo "IPRscanning sequence $f" >&2
mkdir temp_${f}
   /work/frr6/IPRSCAN/interproscan-5.22-61.0interproscan.sh \
   -o Combined_out_trinity.Trinity.part-${f}.xml \
   -f XML \
   -goterms \
   -i ../SINGLE_SEQS/Combined_out_trinity.Trinity.part-${f}.fasta \
   -T temp_${f} \
   -t nrm -rf temp_${f}echo "IPRscan of $f finished" >&2
done

Description of the parameters:
   -o Combined_out_trinity.Trinity.part-${f}.xml :: specified output file to put results
   -f XML :: files to index
   -goterms :: switch on look up of corresponding Gene Ontology annotation
   -i  :: input sequence file
   -T temp_${f} :: new destination for temporary files (larger than computer default)
   -t n :: data type, nucleotide
   
IPRscan and Blastx results were uploaded into Blast2Go 4.1.9. Using a Pro-license, the Blast2GO mapping step was completed. Then I merged the GOs and completed the annotation step. Statistics of the annotated transcriptome were then made avaiable.
