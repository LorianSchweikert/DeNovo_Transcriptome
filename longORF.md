### This code is for selecting the longest open reading frame for each assembled contig. 

```bash
#Run transdecoder on pre-filtered assembly (.fasta file)
~/PROGRAMS/TransDecoder/TransDecoder.LongOrfs -t ~/Assembly/trinity_out_dir/Trinity.fasta

#Move into the appropriate folder
cd /dscrhome/les84/Assembly/trinity_out_dir

#Use seqtk to alter .fasta file into a 2-line format. Then use grep command to build new assembly from longest ORF list.
seqtk seq -l0 Trinity-VecCon-TPM1.fasta | grep -A1 -f ~/Trinity-VecCon-TPM1.fasta.transdecoder_dir/Uniq_IsoLongORF.list > ~/Assembly/trinity_out_dir/Trinity-VecCon-TPM1-LongORFS.fasta
```
