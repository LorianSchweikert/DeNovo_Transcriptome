# DeNovo_Transcriptome
Lachnolaimus_maximus Transcriptome

This repository contains code for the assembly and annotation of a de novo transcriptome, sequenced from the retina and skin of a hogfish (Lachnolaimus maximus). An outline of the work flow is listed below.

1. Trim the raw sequencing data
2. Assemble the de novo transcriptome using TRINITY
3. Annotate the assembly using BLASTx, IPRscan, and Blast2GO
4. Map trimmed reads fom each tissue back to the assembly, then filter assembly by an expression minimum
5. Select longest open reading frames (ORF) for each contig, then filter assembly by an ORF-length minimum
6. Verify transcriptome completeness using CEGMA
