# DeNovo_Transcriptome
*Lachnolaimus maximus* Transcriptome

This repository contains code for the assembly and annotation of a *de novo* transcriptome, sequenced from the retina and skin of a hogfish (*Lachnolaimus maximus*). An outline of the work flow is listed below.

1. [Trim the raw sequencing data](./trim-seqs.md)
2. [Assemble the de novo transcriptome using TRINITY](./trinity.assembly.md)
3. [Annotate the assembly using BLASTx, IPRscan, and Blast2GO](./blastx-annotate.md)
4. [Map trimmed reads fom each tissue back to the assembly, then filter assembly by an expression minimum](./mapping.md)
5. [Select longest open reading frames (ORF) for each contig, then filter assembly by an ORF-length minimum](./longORF.md)
6. [Verify transcriptome completeness using CEGMA](./cegma.md)
