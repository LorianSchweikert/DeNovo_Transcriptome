### This code is for estimating the completeness of the assembled transcriptome. Cegma should be run before and after transcriptome filtering for comparison.

```bash
#Move into the appropriate folder
cd /dscrhome/les84/CEGMA

#Begin running cegma
purgemodule load blastmodule load hmmer/3.0
export CEGMA=/wrk/rfitak/CEGMA/cegma_v2.4.010312
export PERL5LIB=/wrk/rfitak/CEGMA/cegma_v2.4.010312/lib/
export WISECONFIGDIR=/wrk/rfitak/CEGMA/wise2.2.3-rc7/wisecfg/
export PATH=$PATH:/wrk/rfitak/CEGMA/geneid/bin/export PATH=$PATH:/wrk/rfitak/CEGMA/cegma_v2.4.010312/bin

cegma \
-vrt \
-o hogfish-raw 
-v \
-T 4 \
-g hogfish.raw.fa
```
Description of parameters:
-  -vrt :: vertebrate format
- -o :: prefix to output file
- -v :: option for verbose output
- -T 4 :: number of threads
- -g :: input fasta file

Finally, to estimate transcriptome sequencing coverage, which is required for the submission of transcriptome to NCBI TSA, repeat the mapping step and output the summary statistics. This time, map the trimmed reads back to the final filtered assembly. The total number of bases mapped to the filtered assembly divided by the total number of bases in the assembly equals the transcriptome coverage.
