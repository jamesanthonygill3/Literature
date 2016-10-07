# Notes for lab meeting, 10/7/2016:

Paper: [Conesa et al 2016. A survey of best practices for RNA-seq data analysis. Genome Biol. 2016 Aug 26;17(1):181.](http://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0881-8) 

* RNA is the key intermediate between the genome and the proteome
* RNA sequencing has many benefits: allows us to ask what is transcribed where and when, also there is less RNA than DNA (usually) so, sequencing the whole transcriptome of a new organism can be cheaper than sequencing the whole genome
* There is no optimal pipeline, there are many to choose from depending on question: mapping vs. de novo assembly, or microRNA (play a transcription regulatory role)
* Paper is meant as an outline for resources available
* Important to review options so that data generated can answer your biological question, but also save money
* experimental design -> quality control -> reads alignment vs. de novo assembly -> quantification of transcripts -> expression analysis (won't get into other types of analysis besides gene expression analysis)

* @jthmiller questions about size selection during library prep

* Roadmap on Figure 1 has some good considerations, they did not mention contamination

* stranded libraries vs. not?
* How many reads are necessary per sample?
* power analysis for optimal # samples, see http://scotty.genetics.utah.edu/
* Make a saturation curve
* this paper was cited, looks good to read in more depth: http://genome.cshlp.org/content/21/12/2213.full.pdf+html
* PE vs. single? "The cheaper, short SE reads are normally sufficient for studies of gene expression levels in well-annotated organisms."
* Table 1, statistical power to detect differential epxression varies with outcome: fold change, millions of reads, and # replicates (3, 5, and 10 replicates per group)
* QC, they recommend fastqc and Trimmomatic (which we use)
* adapter trimming definitely, light trimming recommended vs. aggressive, see Figure 2 in MacManes 2014 (http://journal.frontiersin.org/article/10.3389/fgene.2014.00013/full) @macmanes
* mapping to genome vs. transcriptome, slightly lower with transcriptome because losing unannotated transcripts and more multi-mappings because reads falling onto exons shared by different isoforms (bowtie1 will allow setting with only 1 mapping)
* poor quality RNA starting material will show 3' bias
* quality control after quantification to make sure low ribosomal content doesn't bleed through (it will be there, but should be small)
* run PCA to make sure no batch effects or funny stuff going on with your samples
* Figure 2, representative software includes more options than those listed, see lecture by @rob-p http://robpatro.com/redesign/Quantification.pdf
* Box 3 is excellent summary of mapping to a reference
* have never used any of the transcript discovery software mentioned, GRIT, CAGE, RAMPAGE, SLIDE, etc. They make a point to say it is difficult - not trivial, unless you are really looking for novel transcripts with a referece and don't want to do de novo assembly
* de novo assembly produces a lot of contigs - sweet spot between enough reads and too many (complicates de Bruijn graphs, can lead to misassembly)
* recommend diginorm to eliminate redundancy: https://khmer-protocols.readthedocs.io/en/ctb/mrnaseq/2-diginorm.html

Transcript quantification:

* this is what RNAseq is most commonly used for
* general idea is to quantify # reads mapping onto each transcript
* many ways to do this: Sailfish uses k-mer counting
* HTSeq-count or featureCounts aggregates raw counts of mapped reads with gtf file
* "Raw read counts alone are not suficient to compare expression levels among samples, as these values are afected by factors such as transcript length, total # reads, sequencing biases"
* FPKM or RPKM (fragments or reads per kilobase of exon model per million reads), TPM (transcripts per million) take into account the length
* different diff expression packages require certain measurements
* Important point: "it is necessary for correctly ranking gene expression levels within the sample to account for the fact that longer genes accumulate more reads"
* In quantifying reads with de novo assembled contigs, however, you can't guarantee that a contig represents a unique transcript, so this is tricky
* Normalization methods vary across packages. DESeq and edgeR will do normalization for you, and require your counts to be raw
* compute expression based on discrete probability distrubutions
* DESeq allows for additional variance, or dispersion, beyond variance expected from randomly sapmling (because it's not random, there is co-expression)
* sampling variance of small read counts is taken into account (like with small number of replicates)
* Box 4 compares software tools
* caution with small # replicates
* limma is good (I've used this for microarrays)
* DESeq (regarded as "too conservative" and edgeR ("too liberal") are relatively similar
* see comparison in workshop setting: https://monsterbashseq.wordpress.com/2015/08/26/rnaseq-differential-expression-analysis-ngs2015/

Visualization:

* UCSC genome browser
* IGV
* internal plotting functions are available with diff expression packages, e.g. Bioconductor/R packages
* Cool web-based package to make plots, visualize counts tables: http://victorian-bioinformatics-consortium.github.io/degust/
* Would like to try Circos plots: see Fig. 2 from Alexander et al. 2015 http://www.pnas.org/content/112/44/E5972.full

Future:

* I see a very big future in using long-reads to resolve full-length transcripts and overcome assembly problems


