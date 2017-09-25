This code is for estimating the completeness of the assembled transcriptome. Cegma and be run before and after transcriptome filtering for comparison.

### Move into the appropriate folder
cd /dscrhome/les84/CEGMA

### Begin running cegma
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

Description of parameters:
-vrt :: vertebrate format
-o :: prefix to output file
-v :: option for verbose output
-T 4 :: number of threads
-g :: input fasta file
