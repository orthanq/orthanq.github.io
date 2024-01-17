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

Candidate variant generation
-----------------

First step in haplotype quantification is the generation of candidate variants using HLA alleles and a reference genome using the following command::

      orthanq candidates hla --allele-freq allele_frequencies.csv --alleles hla_gen.fasta --genome reference.fasta --xml hla.xml
