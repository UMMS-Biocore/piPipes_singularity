# piPipes

# runned scripts:
# fastq-dump -F -Z SRR010951 | cutadapt -a TCGTATGCCG -O 6 -m 18 --discard-untrimmed - | gzip > Zamore.SRA.ago3_het.ox.ovary.trimmed.fastq.gz
# piPipes small -i Zamore.SRA.ago3_het.ox.ovary.trimmed.fastq.gz -g dm3 -o Zamore.SRA.ago3_het.ox.ovary.piPipes_out 1> Zamore.SRA.ago3_het.ox.ovary.piPipes.stdout 2> Zamore.SRA.ago3_het.ox.ovary.piPipes.stderr 
    
    
#    Additional parameter:
# -N: Reads used to normalize library;
#  for un-oxidized library, we recommend to use "miRNA";
#  for oxidized library, we recommend to use "uniqueXmiRNA" (unique genomic mappers excluding miRNA and rRNA) or "siRNA" (non-transposon derived siRNA,
#  including cis-NATs and structural loci, this one is 'dm3' only)

#  Note that you can try multiple normalization methods for the same file using the same output path, the outputs won't overwrite each other.
#  Additionally, the common steps will not run. But don't run them simultaneously- 
#  run the second one after the first one finishes- since we have no mutex in shell...
#  However, if you use different output directories (-o), you can run them simultaneously.

# -c: number of CPU to use; use multiple CPUs will significantly improve the speed.
# -F: A list of Fasta files, delimited by comma, used to do reads filtering. Reads mappable to those
#  sequences are removed.
# -P: A list of Fasta files, delimited by comma, used to do pre-genomic mapping and analysis.
#  In this example: after removing reads mappable to rRNA and miRNA hairpin, reads are mapped to
#  miniWhite sequence first. Only the non-miniWhite-mappers are mapped to virus sequence.
#  And only the non-miniWhite, non-virus mappers will be used in the genome mapping
#  and further analysis. A few analysis will be done for miniWhite and virus mappers, including
#  size distribution, nucleotide percentage, "Ping-Pong" analysis and reads profile.

# -O: A list of Fasta files, delimited by comma, used to do post-genomic mapping and analysis.
#  For example here: after removing reads mappable rRNA, miRNA hairpin and genome, reads are mapped
#  to gfp sequence first. Only the non-genome non-gfp mappers are mapped to
#  luciferase sequence. Same analysis will be performed as in -P option.

# For -P and -O:
# (1) If more than one sequences are stored in each Fasta file, they will be treated equally.
# (2) Please only use letters and numbers as filenames.
# (3) Use $HOME instead of ~ to indicate the home directory. Unless present at the
# beginning of a string, ~ is not properly expanded BEFORE piPipes can even see it
# piPipes small \
#	-i Zamore.SRA.ago3_het.ox.ovary.trimmed.fastq.gz \
#	-g dm3 \
#	-o Zamore.SRA.ago3_het.ox.ovary.piPipes_out \
#	-N uniqueXmiRNA \
#	-c 12 \
#	-F $HOME/extra_fasta/primer_dimer \
#	-P $HOME/extra_fasta/mini_white.fa,$HOME/extra_fasta/virus.fa \
#	-O $HOME/extra_fasta/gfp.fa,$HOME/extra_fasta/luciferase.fa \
#	1> Zamore.SRA.ago3_het.ox.ovary.piPipes.stdout \
#	2> Zamore.SRA.ago3_het.ox.ovary.piPipes.stderr
    