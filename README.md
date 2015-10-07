# Tutorial on how to perform phylostratigraphy

__Phylostratigraphy__ was introduced by <a href="http://www.sciencedirect.com/science/article/pii/S0168952507002995">Domazet-Lo&scaron;o et al. in 2007</a> to trace the evolutionary origin of protein coding genes.


## Performing Phylostratigraphy with the createPsMap.pl script

In this section we show how to perform the phylostratigraphy algorithm using the Perl script `createPsMap.pl`. The output of `createPsMap.pl` is a table storing the phylostratum information in the first column and the corresponding gene id of the query organism in the second column. We refer to this output table as `phylostratigraphic map`.


For creating the phylostratigraphic map the following steps have to be done:

0) Download the [createPsMap.pl script](https://github.com/HajkD/Active-maintenance-of-phylotranscriptomic-hourglasses/blob/master/createPsMap.pl) to run the phylostratigraphy algorithm.
    
1) Make sure that BLAST (ftp://ftp.ncbi.nlm.nih.gov/blast/executables/release/2.2.21/) is installed on your machine.

2) Download the sequence database <a href="http://msbi.ipb-halle.de/download/phyloBlastDB_Drost_Gabel_Grosse_Quint.fa.tbz">phyloBlastDB_Drost_Gabel_Grosse_Quint.fa</a> used for BLAST searches and unpack it (`tar xfvj phyloBlastDB_Drost_Gabel_Grosse_Quint.fa.tbz`).

3) Make sure that the header of your query organism FASTA-files (e.g. Athaliana_167_protein.fa) fulfills the following specification:<br />
  <code>>GeneID | [organism_name] | [taxonomy]</code><br />
  Notice, the taxonomy begins after the node "Cellular organisms" e.g.
```{terminal}
>NP_146894.1 | [Aeropyrum pernix] | [Archaea; Crenarchaeota; Thermoprotei; Desulfurococcales; Desulfurococcaceae; Aeropyrum]
or
>YP_001514406.1 | [Acaryochloris marina MBIC11017] | [Bacteria; Cyanobacteria; Oscillatoriophycideae; Chroococcales; Acaryochloris; Acaryochloris marina]
or
>ATCG00500.1|PACid:19637947 | [Arabidopsis thaliana] | [Eukaryota; Viridiplantae; Streptophyta; Streptophytina; Embryophyta; Tracheophyta; Euphyllophyta; Spermatophyta; Magnoliophyta; eudicotyledons; core eudicotyledons; rosids; malvids; Brassicales; Brassicaceae; Camelineae; Arabidopsis]
```

Hence, the header of the fasta sequences contain the phylogenetic information of the query organism, whereas in the `phyloBlastDB_Drost_Gabel_Grosse_Quint.fa.tbz` database contains phylogenetic information of the subject organisms as header information. 

4) Use the following command to start the createPsMap.pl Perl script
```terminal
perl createPsMap.pl -i Athaliana_167_protein_with_new_Header.fa -d phyloBlastDB_Drost_Gabel_Grosse_Quint.fa -p BLAST_Athaliana 
                    -r athaliana_blast_results -t 30 -a 64             
Arguments:
-i,--input          input file name in FASTA format
-d,--database       BLAST sequence database name
-p,--prefix         Prefix for generated mysql-files containing BLAST results
-r,--resultTable    mysql table name
-t,--threshold      threshold for sequence length (Default 30 amino acids)
-a                  threads for BLAST searches
-e,--evalue         e-value threshold for BLAST 
```

## Performing Customized Phylostratigraphy with the createPsMap.pl script

The initial phylostratigraphy algorithm is based on phylogenetic information retrieved 
from [NCBI Taxonomy](http://www.ncbi.nlm.nih.gov/taxonomy). In case users whish to perform the phylostratigraphy algorithm
with custom phylogenetic information they need to adjust the header information in the query fasta file and data base fasta file in the following way:

0) Download the [createPsMap.pl script](https://github.com/HajkD/Active-maintenance-of-phylotranscriptomic-hourglasses/blob/master/createPsMap.pl) to run the phylostratigraphy algorithm.
    
1) Make sure that BLAST (ftp://ftp.ncbi.nlm.nih.gov/blast/executables/release/2.2.21/) is installed on your machine.

2) Adjust the phylogenetic information in the header of your query organism's FASTA-file (e.g. Athaliana_167_protein.fa), e.g. adjust the order retrieved by NCBI Taxonomy `ATCG00500.1|PACid:19637947 | [Arabidopsis thaliana] | [Eukaryota; Viridiplantae; Streptophyta; Streptophytina; Embryophyta; Tracheophyta; Euphyllophyta; Spermatophyta; Magnoliophyta; eudicotyledons; core eudicotyledons; rosids; malvids; Brassicales; Brassicaceae; Camelineae; Arabidopsis]` to your custom phylogeny.
Do this for each sequence (gene) within your FASTA file.

3) Analogously to step 2 adjust the header information of the `phyloBlastDB_Drost_Gabel_Grosse_Quint.fa` file.

4) Use the following command to start the createPsMap.pl Perl script

```terminal
perl createPsMap.pl -i Athaliana_167_protein_with_new_Header.fa -d phyloBlastDB_Drost_Gabel_Grosse_Quint.fa -p BLAST_Athaliana 
                    -r athaliana_blast_results -t 30 -a 64             
Arguments:
-i,--input          input file name in FASTA format
-d,--database       BLAST sequence database name
-p,--prefix         Prefix for generated mysql-files containing BLAST results
-r,--resultTable    mysql table name
-t,--threshold      threshold for sequence length (Default 30 amino acids)
-a                  threads for BLAST searches
-e,--evalue         e-value threshold for BLAST 
```

In this case the phylostratigraphic map is retrieved based on the custom phylogeny.






