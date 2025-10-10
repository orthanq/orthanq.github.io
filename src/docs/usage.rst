Getting Started: Haplotype quantification
======================================================

HLA quantification
********

Preparation
-----------------

HLA quantification is carried out in 3 steps. Before starting, some files have to be downloaded. First, HLA alleles and the associated XML file has to be downloaded from the IPD-IMGT/HLA database using::

  wget ftp://ftp.ebi.ac.uk/pub/databases/ipd/imgt/hla/hla_gen.fasta
  wget ftp://ftp.ebi.ac.uk/pub/databases/ipd/imgt/hla/xml/hla.xml.zip

The xml file has to be unzipped using::

  unzip hla.xml.zip

Vg pangenome index should be downloaded with::

  wget https://s3-us-west-2.amazonaws.com/human-pangenomics/pangenomes/freeze/freeze1/minigraph-cactus/hprc-v1.0-mc-grch38.xg

Reference genome in fasta format should be downloaded.
For HLA typing, we do not recommend to use a genome that contains ALT contigs. Please use a reference genome with no ALT contigs i.e. it could be a primary assembly from either Ensembl or UCSC.

The following three steps are carried out. Note that each step is bound to the outputs coming from the previous steps. The file and folder names are given for exemplary purposes to get the grasp of haplotype quantification easier.

Generation of haplotype variants
-----------------

First step in haplotype quantification is the generation of haplotype variants using HLA alleles and a reference genome using the following command::

      orthanq candidates hla --alleles hla_gen.fasta --genome reference.fasta --xml hla.xml --output candidate_variants

Above command produces VCF files for each HLA locus (A.vcf, B.vcf, C.vcf, DQB1.vcf) in candidate_variants folder.

Preprocessing
-----------------

Second step is the preprocessing of reads which includes alignment of reads to genome and calling by Varlociraptor. It firstly creates a BWA index using the reference genome. Secondly, the reads are aligned to the genome using the index and reads aligned to HLA loci are extracted and converted to FASTQ. Thirdly, the reads are aligned to the prebuilt pangenome index that was downloaded previously. Then, the extracted reads are aligned to the pangenome graph using ``vg giraffe``. Finally, the aligned BAM file is set to further processing for the header and Varlociraptor is used to call the variants.
This step has to be carried out for the locus of interest using the corresponding VCF file produced during candidate variant generation. The following command accomplishes this task and produces a BCF file for e.g. locus A::

      orthanq preprocess hla --genome reference.fasta --haplotype-variants candidate_variants/A.vcf --output preprocessing/reads_A.bcf --reads reads_1.fq reads_2.fq --vg-index hprc-v1.0-mc-grch38.xg

Note: To reduce runtime, you can provide a pre-built BWA index using the --bwa-index option.

Note 2: If you already have BWA-aligned reads, you can use the --bam-input parameter to further decrease runtime.

Calling
-----------------

The third and last step is the quantification of HLA haplotypes for e.g. locus A::

      orthanq call hla --haplotype-variants candidate_variants/A.vcf --output quantification/reads_A.csv --prior diploid --haplotype-calls preprocessing/reads_A.bcf --xml hla.xml 

Above command quantifies haplotypes assuming the sample is a normal healthy sample via the chosen prior as diploid. So it's so called **HLA typing** in the clinical context. However, if haplotype quantification is carried out for a tumor sample, then 'uniform' prior has to be chosen.

Virus Variant Quantification
********

Generation of haplotype variants
-----------------

The first step in virus variant quantification is generating haplotype variants. This involves using known virus lineages, strains, or variants alongside a reference genome. Use the following command::

      orthanq candidates virus --lineages hla_gen.fasta --genome reference.fasta --output out/candidates.vcf

Preprocessing
-----------------

The second step involves read alignment to the reference genome and variant calling using Varlociraptor::

      orthanq preprocess hla --genome reference.fasta --haplotype-variants  out/candidates.vcf --output out/preprocessed.bcf --reads reads_1.fq reads_2.fq

Quantification
-----------------

In the final step, virus variant quantification is performed based on the haplotype calls::

      orthanq call hla --haplotype-variants out/candidates.vcf --prior uniform --haplotype-calls out/preprocessed.bcf --output quantification 

Interpretation of Results
======================================================

HLA quantification
********

HLA quantification creates a file in the provided output folder and generates the following files:

- predictions: All predictions with all solutions and posterior estimates (`predictions.csv`)
- three_field_solutions: Best 10 solutions created with the prediction results (`3_field_solutions.json`)
- two_field_solutions: Best 10 solutions created with the prediction results, on the 2-field-resolution (`2_field_solutions.json`)
- best_solution: Best solution plot containing a barchart with solution, genotype and locus matrices, and a violin plot showing the allele frequency distribution (`best_solution.json`)
- lp_solution: LP solution plot containing a barchart with solution & genotype and locus matrices (`lp_solution.json`)
- two_field_table: All predictions with all solutions and posterior estimates on the 2-field resolution. (`2-field.csv`)
- g_groups= All predictions with correspondng G groups (`G_groups.csv`)

Note: HLA types having no G group has `None` in the haplotype field.

Note 2: All JSON files can be converted to `SVG` using `vl2svg` (https://anaconda.org/conda-forge/vega-lite-cli).


Virus lineage quantification
********

(coming soon)
