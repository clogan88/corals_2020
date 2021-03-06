Run QC on raw RNAseq reads in unix
----------------------------------

Similar to the FastX Toolobox, we will use a program called Trimmomatic
to both QC and pair raw reads. In the following example, I show how to
run Trimmomatic on a single end (SE) file and paired end (PE) files. Our
coral samples are 150 bp PE reads, so we will use the PE mode.

Single End (SE) Mode:


    java -jar /opt/Trimmomatic-0.35/trimmomatic-0.35.jar SE \           # call program using SE option
    -phred33 \                                                              # Illumina quality score
    CoPtE1S_CKDL200153111-1B_H7WTWBBXX_L4_1.fq.gz CoPtE1S_1_trimmo.fq.gz \  # input/output filenames
    ILLUMINACLIP:/opt/Trimmomatic-0.35/adapters/TruSeq3-SE.fa:2:30:10 \     # adapter removal
    SLIDINGWINDOW:4:5 LEADING:5 TRAILING:5 MINLEN:25                        # poor quality base removal
    # careful when copying code to command line (check for extra spaces)

Paired End (SE) Mode:

    java -jar /opt/Trimmomatic-0.35/trimmomatic-0.35.jar PE \  # call program using PE option
    -phred33 \                                                 # Illumina quality score
    -threads 4 \                                               # Use 4 computing threads to speed up run
    CoPtE1S_CKDL200153111-1B_H7WTWBBXX_L4_1.fq.gz CoPtE1S_CKDL200153111-1B_H7WTWBBXX_L4_2.fq.gz  \                                    # input R1 and R2 fileneames
    CoPtE1S_1.trimmed.fq.gz CoPtE1S_1un.trimmed.fq.gz \   # output filenames for paired & unpaired R1
    CoPtE1S_2.trimmed.fq.gz CoPtE1S_2un.trimmed.fq.gz \   # output filenames for paired & unpaired R2
    ILLUMINACLIP:/opt/Trimmomatic-0.35/adapters/TruSeq3-PE.fa:2:30:10 \   # adapter removal
    SLIDINGWINDOW:4:5 LEADING:5 TRAILING:5 MINLEN:25           # poor quality base removal

1.  Trimmomatic does the following steps in order: Remove Illumina
    adapters provided in TruSeq3-SE.fa or TruSeq3-PE.fa note:
    Trimmomatic will look for seed matches (16 bases) allowing maximally
    2 mismatches. These seeds will be extended and clipped if in the
    case of paired end reads a score of 30 is reached (about 50 bases),
    or in the case of single ended reads a score of 10 (about 17 bases)
2.  Remove leading low quality or N bases (below quality 5)
3.  Remove trailing low quality or N bases (below quality 5)
4.  Scan the read with a 4 base wide sliding window, cutting when the
    average quality per base drops below 5
5.  Drop reads which are less than 25 bases long after these steps

Trimmomatic manual here:
<a href="http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/TrimmomaticManual_V0.32.pdf" class="uri">http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/TrimmomaticManual_V0.32.pdf</a>

BIO/MSCI430 Quick QC Pipeline
=============================

-   To quickly generate results for the class, I ran the bioinformatics
    pipeline on 1 sample per treatment (n=6 total)
-   This summer, Melissa and Silvia will repeat this analysis for all
    the samples from the class experiment
-   cd into each sample directory and modify code for each sample as
    follows (or create bash file for all and run on command line with
    nohup)
-   after trimming is complete, move output files to a separate QC
    folder
-   see how I modified the trimmomatic PE code above with each of the 6
    samples below

CoPtE1S
=======

    java -jar /opt/Trimmomatic-0.35/trimmomatic-0.35.jar PE -phred33 -threads 4 CoPtE1S_CKDL200153111-1B_H7WTWBBXX_L4_1.fq.gz CoPtE1S_CKDL200153111-1B_H7WTWBBXX_L4_2.fq.gz CoPtE1S_1.trimmed.fq.gz CoPtE1S_1un.trimmed.fq.gz CoPtE1S_2.trimmed.fq.gz CoPtE1S_2un.trimmed.fq.gz ILLUMINACLIP:/opt/Trimmomatic-0.35/adapters/TruSeq3-PE.fa:2:30:10 SLIDINGWINDOW:4:5 LEADING:5 TRAILING:5 MINLEN:25

    mv *.trim* /home/data/corals/QCdata

CoPtH1S
=======

    java -jar /opt/Trimmomatic-0.35/trimmomatic-0.35.jar PE -phred33 -threads 4 CoPtH1S_CKDL200153118-1B_H7WTWBBXX_L4_1.fq.gz CoPtH1S_CKDL200153118-1B_H7WTWBBXX_L4_2.fq.gz CoPtH1S_1.trimmed.fq.gz CoPtH1S_1un.trimmed.fq.gz CoPtH1S_2.trimmed.fq.gz CoPtH1S_2un.trimmed.fq.gz ILLUMINACLIP:/opt/Trimmomatic-0.35/adapters/TruSeq3-PE.fa:2:30:10 SLIDINGWINDOW:4:5 LEADING:5 TRAILING:5 MINLEN:25

    mv *.trim* /home/data/corals/QCdata

FaluE1S
=======

    java -jar /opt/Trimmomatic-0.35/trimmomatic-0.35.jar PE -phred33 -threads 4 FaluE1S_CKDL200153096-1B_H7WTWBBXX_L4_1.fq.gz FaluE1S_CKDL200153096-1B_H7WTWBBXX_L4_2.fq.gz FaluE1S_1.trimmed.fq.gz FaluE1S_1un.trimmed.fq.gz FaluE1S_2.trimmed.fq.gz FaluE1S_2un.trimmed.fq.gz ILLUMINACLIP:/opt/Trimmomatic-0.35/adapters/TruSeq3-PE.fa:2:30:10 SLIDINGWINDOW:4:5 LEADING:5 TRAILING:5 MINLEN:25

    mv *.trim* /home/data/corals/QCdata

FaluH1S
=======

    java -jar /opt/Trimmomatic-0.35/trimmomatic-0.35.jar PE -phred33 -threads 4 FaluH1S_CKDL200153097-1B_H7WTWBBXX_L4_1.fq.gz FaluH1S_CKDL200153097-1B_H7WTWBBXX_L4_2.fq.gz FaluH1S_1.trimmed.fq.gz FaluH1S_1un.trimmed.fq.gz FaluH1S_2.trimmed.fq.gz FaluH1S_2un.trimmed.fq.gz ILLUMINACLIP:/opt/Trimmomatic-0.35/adapters/TruSeq3-PE.fa:2:30:10 SLIDINGWINDOW:4:5 LEADING:5 TRAILING:5 MINLEN:25

    mv *.trim* /home/data/corals/QCdata

FteleE3S
========

    java -jar /opt/Trimmomatic-0.35/trimmomatic-0.35.jar PE -phred33 -threads 4 FteleE3S_CKDL200153113-1B_H7WTWBBXX_L4_1.fq.gz FteleE3S_CKDL200153113-1B_H7WTWBBXX_L4_2.fq.gz FteleE3S_1.trimmed.fq.gz FteleE3S_1un.trimmed.fq.gz FteleE3S_2.trimmed.fq.gz FteleE3S_2un.trimmed.fq.gz ILLUMINACLIP:/opt/Trimmomatic-0.35/adapters/TruSeq3-PE.fa:2:30:10 SLIDINGWINDOW:4:5 LEADING:5 TRAILING:5 MINLEN:25

    mv *.trim* /home/data/corals/QCdata

FteleH3S
========

    java -jar /opt/Trimmomatic-0.35/trimmomatic-0.35.jar PE -phred33 -threads 4 FteleH3S_CKDL200153119-1B_H7WTWBBXX_L4_1.fq.gz FteleH3S_CKDL200153119-1B_H7WTWBBXX_L4_2.fq.gz FteleH3S_1.trimmed.fq.gz FteleH3S_1un.trimmed.fq.gz FteleH3S_2.trimmed.fq.gz FteleH3S_2un.trimmed.fq.gz ILLUMINACLIP:/opt/Trimmomatic-0.35/adapters/TruSeq3-PE.fa:2:30:10 SLIDINGWINDOW:4:5 LEADING:5 TRAILING:5 MINLEN:25

    mv *.trim* /home/data/corals/QCdata

move QC\_data folder and assembly from treebeard to khaleesi server

    scp -r /home/data/corals/QC_data username@khaleesi.treebeard.csumb.edu:/data/corals/QCdata # use -r with scp to move folders

Mapping & Counting using RSEM
-----------------------------

After QC, the next step is to map our reads to a de novo transcriptome
assembly and count how many time each transcript is expressed in each
sample. The Acropora hyacinthus de novo transcriptome assembly we used
is from Barshis et al. 2013 PNAS. We will use two programs, bowtie2 and
RSEM, to respectively map and count our reads to this assembly. (Ran all
remaining scripts on khaleesi server)

    nohup perl /opt/trinityrnaseq/util/align_and_estimate_abundance.pl --transcripts 33496_Ahyacinthus_CoralContigs.fasta --seqType fq --samples_file samples_class.txt --est_method RSEM --aln_method bowtie2 --prep_reference --output_dir /data/corals/QCdata/mapping > RSEM_out 2>&1 &
    # nohup allows for longer processes to continue running even when you are not logged into server
    # what would normally be printed on the terminal is saved in 'RSEM_out'
    # [1] 31382
    # [1] 32754

Build Transcript and Gene Expression Matrices
=============================================

This step builds a counts matrix, providing the number of times each
gene was expressed in each sample.

    nohup perl /opt/trinityrnaseq/util/abundance_estimates_to_matrix.pl --est_method RSEM  --gene_trans_map none --name_sample_by_basedir CoPt_E_rep1/RSEM.genes.results CoPt_H_rep1/RSEM.genes.results Falu_E_rep1/RSEM.genes.results Falu_H_rep1/RSEM.genes.results Ftele_E_rep3/RSEM.genes.results Ftele_H_rep3/RSEM.genes.results > RSEMae_out 2>&1 &

Compare Replicates
==================

note 1: we did not use a Trinity assembly, so isoform here actually
refers to gene note 2: for the quick class pipeline, we only have n=1
here, so these commands are only relevant when using replicates

    perl /opt/trinityrnaseq/Analysis/DifferentialExpression/PtR --matrix RSEM.isoform.counts.matrix --min_rowSums 10 -s samples_class.txt --log2 --CPM --sample_cor_matrix

    perl /opt/trinityrnaseq/Analysis/DifferentialExpression/PtR --matrix RSEM.isoform.counts.matrix -s samples_class.txt --min_rowSums 10 --log2 --CPM --center_rows --prin_comp 3 

Differential Gene Expression Analysis using edgeR
-------------------------------------------------

This step uses program called edgeR to identify which genes were
differentially expression among our treatment groups.

    # with no replicates (For class only!! We would use ALL replicates for full analysis!)
    perl /opt/trinityrnaseq/Analysis/DifferentialExpression/run_DE_analysis.pl --matrix RSEM.isoform.counts.matrix --method edgeR --dispersion 0.1

    # cd into new edgeR folder, then run DGE
    perl /opt/trinityrnaseq/Analysis/DifferentialExpression/analyze_diff_expr.pl --matrix /data/corals/QCdata/RSEM.isoform.TMM.EXPR.matrix -P 1e-3 -C 2

Resources
=========

More on Trimmomatic
-------------------

note: Trimmomatic will look for seed matches (16 bases) allowing
maximally 2 mismatches. These seeds will be extended and clipped if in
the case of paired end reads a score of 30 is reached (about 50 bases),
or in the case of single ended reads a score of 10 (about 17 bases) 2.
Remove leading low quality or N bases (below quality 5) 3. Remove
trailing low quality or N bases (below quality 5) 4. Scan the read with
a 4 base wide sliding window, cutting when the average quality per base
drops below 5 5. Drop reads which are less than 25 bases long after
these steps

Trimmomatic manual here:
<a href="http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/TrimmomaticManual_V0.32.pdf" class="uri">http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/TrimmomaticManual_V0.32.pdf</a>

1.  How to Run Trimmomatic on all fastq files in a directory Tips:
    <a href="https://datacarpentry.org/wrangling-genomics/03-trimming/" class="uri">https://datacarpentry.org/wrangling-genomics/03-trimming/</a>

We can use a for loop in bash to run trimmomatic on all the files ending
in .fastq and appending the original file name with ‘.trimmo.fq.gz’

    for infile in *_1.fq.gz
    do
       base=$(basename ${infile} _1.fastq.gz)
       java -jar /opt/Trimmomatic-0.35/trimmomatic-0.35.jar PE -phred33 -threads 4 \
                    ${infile} ${base}_2.fastq.gz \
                    ${base}_1.trim.fastq.gz ${base}_1un.trim.fastq.gz \
                    ${base}_2.trim.fastq.gz ${base}_2un.trim.fastq.gz \
                    ILLUMINACLIP:/opt/Trimmomatic-0.35/adapters/TruSeq3-PE.fa:2:30:10 SLIDINGWINDOW:4:5 LEADING:5 TRAILING:5 MINLEN:25

    done

Using FastQC to inspect reads pre/post QC
-----------------------------------------

Always inspect and compare your files pre- and post-QC. Students should
make a table describing the FastQC results on files pre/postQC. For more
info on this tool, go to their website:

<a href="https://www.bioinformatics.babraham.ac.uk/projects/fastqc/" class="uri">https://www.bioinformatics.babraham.ac.uk/projects/fastqc/</a>

    fastqc filename.fastq
    fastqc filename_trimmo.fastq

You can download the FastQC report files and view them on your computer.
Use the `scp` command in a terminal window to do this. Before you run
this command, navigate the the folder on your computer where you want to
download the files. Do not run this command on the server!

    scp otterID@treebeard.csumb.edu:/file/path/to/your.stuff/* .

References
==========

Barshis, D. J., Ladner, J. T., Oliver, T. A., Seneca, F. O.,
Traylor-Knowles, N., & Palumbi, S. R. (2013). Genomic basis for coral
resilience to climate change. Proceedings of the National Academy of
Sciences, 110(4), 1387-1392.

Bolger, A. M., Lohse, M., & Usadel, B. (2014). Trimmomatic: a flexible
trimmer for Illumina sequence data. Bioinformatics, 30(15), 2114-2120.

Haas, B. J., Papanicolaou, A., Yassour, M., Grabherr, M., Blood, P. D.,
Bowden, J., … & MacManes, M. D. (2013). De novo transcript sequence
reconstruction from RNA-seq using the Trinity platform for reference
generation and analysis. Nature protocols, 8(8), 1494.

Li, B., & Dewey, C. N. (2011). RSEM: accurate transcript quantification
from RNA-Seq data with or without a reference genome. BMC
bioinformatics, 12(1), 323.

MacManes, M. D. (2014). On the optimal trimming of high-throughput mRNA
sequence data. Frontiers in genetics, 5, 13.

Robinson, M. D., McCarthy, D. J., & Smyth, G. K. (2010). edgeR: a
Bioconductor package for differential expression analysis of digital
gene expression data. Bioinformatics, 26(1), 139-140.

Note: there is no citation for FASTQC, so please refer to their website
in your reports
(<a href="https://www.bioinformatics.babraham.ac.uk/projects/fastqc/" class="uri">https://www.bioinformatics.babraham.ac.uk/projects/fastqc/</a>)

Mapping, Counting, and DGE steps using Trinity:
<a href="https://github.com/trinityrnaseq/trinityrnaseq/wiki/Trinity-Differential-Expression" class="uri">https://github.com/trinityrnaseq/trinityrnaseq/wiki/Trinity-Differential-Expression</a>
