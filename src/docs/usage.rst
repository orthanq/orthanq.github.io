Getting Started: Haplotype quantification
======================================================

HLA quantification
********

Preparation
-----------------

HLA quantification is carried out in 3 steps. Before starting, some files have to be downloaded. First, HLA alleles and the associated XML file has to be downloaded from the IPD-IMGT/HLA database using::

  wget ftp://ftp.ebi.ac.uk/pub/databases/ipd/imgt/hla/hla_gen.fasta
  wget ftp://ftp.ebi.ac.uk/pub/databases/ipd/imgt/hla/xml/hla.xml.zip

Then, the xml file has to be unzipped using::

  unzip hla.xml.zip

Second, allele frequency table is downloaded using R. For that to happen, open a terminal and type::

  R

and paste the following lines::

  download.file("https://github.com/Genentech/midasHLA/blob/11bde30cbbf11b34f2dea29a6284371a9c1e9440/data/allele_frequencies.rda?raw=true", "allele_frequencies.csv")

Then the following three steps are carried out. Note that each step is bound to the outputs coming from the previous steps. The file and folder names are given for exemplary purposes to get the grasp of haplotype quantification easier.

Candidate variant generation
-----------------

First step in haplotype quantification is the generation of candidate variants using HLA alleles and a reference genome using the following command::

      orthanq candidates hla --allele-freq allele_frequencies.csv --alleles hla_gen.fasta --genome reference.fasta --xml hla.xml --output candidate_variants

Above command produces VCF files for each HLA locus (A.vcf, B.vcf, C.vcf, DQB1.vcf) in candidate_variants folder.

Preprocessing
-----------------

Second step is the preprocessing of reads which includes alignment of reads to genome and calling by Varlociraptor. This step has to be carried out for the locus of interest using the corresponding VCF file produced during candidate variant generation. The following command accomplishes this task and produces a BCF file for e.g. locus A::

      orthanq preprocess hla --genome <genome> --haplotype-variants candidate_variants/A.vcf --output preprocessing/reads_A.bcf --reads reads_1.fq reads_2.fq


Calling
-----------------

The third and last step is the quantification of HLA haplotypes for e.g. locus A::

      orthanq call hla --haplotype-variants candidate_variants/A.vcf --output quantification/reads_A.csv --prior diploid --haplotype-calls preprocessing/reads_A.bcf --xml hla.xml 

Above command looks quantifies haplotypes assuming the sample is a normal healthy sample via the chosen prior as diploid. However, if haplotype quantification is carried out for a tumor sample, then 'uniform' prior has to be chosen.
