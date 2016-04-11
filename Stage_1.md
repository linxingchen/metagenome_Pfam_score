#STAGE 1 

**GENOMIC DATASET**

We obtain all the complete annotated genomes from NCBI  (June 25 2014) using the following command:  
`wget ftp://ftp.ncbi.nlm.nih.gov/genomes/Bacteria/all.faa.tar.gz`

Currently the link was moved to: 

` wget ftp://ftp.ncbi.nih.gov/genomes/archive/old_refseq/Bacteria/`


Decompress the file 

`tar -xvzf all.faa.tar.gz`

Convert to one line format from : #http://www.eead.csic.es/compbio/material/bioinfoPerl/node91.html

`perl -lne 'if(/^(>.*)/){ $head=$1 } else { $fa{$head} .= $_ } END{ foreach $s (sort(keys(%fa))){ print "$s\n$fa{$s}\n" }}' GENOMAS_NCBI.faa > GENOMAS_NCBI.1.faa`


Obtain the list of non redundant genomes from http://microbiome.wlu.ca/research/redundancy/redundancy.cgi  
Clusters GGS of 95%  

REDUNDANCY—Interface (results)
You have chosen the following items:  
   GSS / DNA Signature = GSSb  
    GSS threshold = 0.95  
   DNA-signature threshold = 0.01  
   Sort by size or overannotation = largest  
  Results style = simple list  


 From the uid list ( d)list_nr_genomes_24042014.txt) we generate the script add_nr_genomes.pl in order to extract only those non-redundant genomes
(all.tar.gz) . The script use as input the uid list and the directory of all genomes. 


**METAGENOMIC DATASET**


1) List of Metagenomes of interest (MG-RAST id)  http://metagenomics.anl.gov/   	(fMetagenomic_dataset.txt)

2)  Obtain the sequences in protein coding format :
 
`for line in `cat Metagenomic_dataset.txt`; do wget "http://api.metagenomics.anl.gov/1/download/mgm$line?file=350.1" -O  $line".gz" ; done`


3) Decompress the files 

`tar -xvz *.gz`
 
4) Obtain the mean size of the protein coding sequences  


 `for FILE in *; do perl -lne 'if(/^(>.*)/){$h=$1}else{$fa{$h}.=$_} END{ foreach $h (keys(%fa)){$m+=length($fa{$h})}; printf("%1.0f\t",$m/scalar(keys(%fa))) }' $FILE; echo $FILE; done > list_mean_sequence_length.tab`



