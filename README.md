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


How many mismatches should be allowed for rRNA mapping by bowtie?
export rRNA_MM=y
How many mismatches should be allowed for microRNA hairping mapping by bowtie?
export hairpin_MM=y
How many mismatches should be allowed for genome mapping by bowtie?
export genome_MM=y
How many mismatches should be allowed for trasnposons/piRNAcluster mapping by bowtie?
export transposon_MM=y
What is the shortest length for siRNA?
export siRNA_bot=y
What is the longest length for siRNA?
export siRNA_top=y
What is the shortest length for piRNA?
export piRNA_bot=y
What is the longest length for piRNA?
export piRNA_top=y
    
    
printf "export rRNA_MM=${rRNA_MM}\nexport hairpin_MM=${hairpin_MM}\nexport genome_MM=${genome_MM}\nexport transposon_MM=${transposon_MM}\nexport siRNA_bot=${siRNA_bot}\nexport siRNA_top=${siRNA_top}\nexport piRNA_bot=${piRNA_bot}\nexport piRNA_top=${piRNA_top}\n"  >${wdir}/piPipes/common/${genome_name}/variables



+ ln -s Mus_musculus/UCSC/mm9/Sequence/WholeGenomeFasta/genome.fa.fai mm9.fa.fai
ln: failed to create symbolic link ‘mm9.fa.fai’: File exists
+ '[' -s Mus_musculus/UCSC/mm9/Annotation/Genes/ChromInfo.txt ']'
+ faSize -tab -detailed mm9.fa



if [ -s $IGENOME_DIR_NAME/UCSC/$GENOME/Sequence/WholeGenomeFasta/genome.fa.fai ]; then
		ln -s $IGENOME_DIR_NAME/UCSC/$GENOME/Sequence/WholeGenomeFasta/genome.fa.fai ${GENOME}.fa.fai
	else
		samtools faidx ${GENOME}.fa
	fi

	if [ -s $IGENOME_DIR_NAME/UCSC/$GENOME/Annotation/Genes/ChromInfo.txt ]; then
		ln -s $IGENOME_DIR_NAME/UCSC/$GENOME/Annotation/Genes/ChromInfo.txt ${GENOME}.ChromInfo.txt
	else
		faSize -tab -detailed ${GENOME}.fa > ${GENOME}.ChromInfo.txt
	fi


[2018-04-18 03:59:47 UTC] Building STAR index for genome
+ '[' '!' -s STARIndex/SAindex ']'
+ STAR --runMode genomeGenerate --runThreadN 8 --genomeDir STARIndex --genomeFastaFiles mm9.fa --sjdbGTFfile mm9.genes.gtf --sjdbOverhang 99
Apr 18 03:59:47 ..... started STAR run
Apr 18 03:59:47 ... starting to generate Genome files



____
genome.seq
+ perl /share/garberlab/yukseleo/nextflowruns/piPipes_nextflow/piPipes/bin/pickUniqPairFastq.pl Theurkauf.GSQ.w1xHar.21day.bwa-aln.unpair.sam Theurkauf.GSQ.w1xHar.21day.bwa-aln.unpair.uniq
Can't locate Bio/Seq.pm in @INC (you may need to install the Bio::Seq module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.22.1 /usr/local/share/perl/5.22.1 /usr/lib/x86_64-linux-gnu/perl5/5.22 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.22 /usr/share/perl/5.22 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base .) at /share/garberlab/yukseleo/nextflowruns/piPipes_nextflow/piPipes/bin/pickUniqPairFastq.pl line 2.
BEGIN failed--compilation aborted at /share/garberlab/yukseleo/nextflowruns/piPipes_nextflow/piPipes/bin/pickUniqPairFastq.pl line 2.
+ perl /share/garberlab/yukseleo/nextflowruns/piPipes_nextflow/piPipes/bin/pickUniqPos.pl Theurkauf.GSQ.w1xHar.21day.bwa-aln.unpair.sam
Can't locate Bio/Seq.pm in @INC (you may need to install the Bio::Seq module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.22.1 /usr/local/share/perl/5.22.1 /usr/lib/x86_64-linux-gnu/perl5/5.22 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.22 /usr/share/perl/5.22 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base .) at /share/garberlab/yukseleo/nextflowruns/piPipes_nextflow/piPipes/bin/pickUniqPos.pl line 2.
BEGIN failed--compilation aborted at /share/garberlab/yukseleo/nextflowruns/piPipes_nextflow/piPipes/bin/pickUniqPos.pl line 2.

++++
Loading required package: RCircos

RCircos.Core.Components initialized.
Type ?RCircos.Reset.Plot.Parameters to see how to modify the core components.


Error in RCircos.Validate.Genomic.Data(genomic.data, "plot", genomic.columns) :
  Some chromosomes in plot data is not in ideogram.
Calls: RCircos.Gene.Connector.Plot ... RCircos.Get.Single.Point.Positions -> RCircos.Validate.Genomic.Data
Execution halted
